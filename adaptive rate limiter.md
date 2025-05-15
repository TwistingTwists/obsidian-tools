
https://www.youtube.com/watch?v=CQvmSXlnyeQ&list=PLj6h78yzYM2O1wlsM-Ma-RYhfT5LKq0XC&index=22

https://github.com/vectordotdev/vector/blob/master/src/sinks/util/adaptive_concurrency/controller.rs


ShrinkableSemaphore 

```mermaid

sequenceDiagram
    participant C as Client Code
    participant SS as ShrinkableSemaphore
    participant MFF as MaybeForgetFuture
    participant TS as Tokio Semaphore

    Note over C,TS: Normal Acquisition Process
    C->>SS: acquire()
    SS->>MFF: create
    activate MFF
    MFF->>SS: lock to_forget mutex
    SS-->>MFF: mutex guard
    
    alt to_forget > 0
        loop while to_forget > 0
            MFF->>TS: poll inner future (acquire_owned)
            TS-->>MFF: OwnedSemaphorePermit
            MFF->>MFF: permit.forget()
            MFF->>MFF: decrement to_forget
            MFF->>TS: create new future for next permit
        end
    end
    
    MFF->>MFF: drop mutex guard
    MFF->>TS: poll inner future (acquire_owned)
    TS-->>MFF: OwnedSemaphorePermit
    MFF-->>C: OwnedSemaphorePermit
    deactivate MFF
    
    Note over C,TS: Shrinking Semaphore Process
    C->>SS: forget_permits(count)
    activate SS
    SS->>SS: lock to_forget mutex
    
    loop for each count
        SS->>TS: try_acquire()
        alt permit acquired
            TS-->>SS: Permit
            SS->>SS: permit.forget()
        else no permit available
            SS->>SS: increment to_forget
        end
    end
    
    SS->>SS: unlock mutex
    deactivate SS
    
    Note over C,TS: Adding Permits Process
    C->>SS: add_permits(count)
    activate SS
    SS->>SS: lock to_forget mutex
    
    alt to_forget >= count
        SS->>SS: decrease to_forget by count
    else to_forget < count
        SS->>TS: add_permits(count - to_forget)
        SS->>SS: reset to_forget to 0
    end
    
    SS->>SS: unlock mutex
    deactivate SS
```


```mermaid
flowchart TD
    %% Style definitions
    classDef publicAPI fill:#c4e3b2,stroke:#1d8900,stroke-width:2px
    classDef internalImpl fill:#d7d7ff,stroke:#0000ff,stroke-width:1px
    classDef futureImpl fill:#ffe6cc,stroke:#d79b00,stroke-width:1px

    subgraph "ShrinkableSemaphore Structure"
        S1["ShrinkableSemaphore"] --> S2["Arc<Semaphore>"]
        S1 --> S3["Mutex<usize> to_forget"]
    end

    subgraph "Public API"
        A1["new<br/><i>Create new semaphore</i>"]:::publicAPI
        A2["acquire<br/><i>Get permit asynchronously</i>"]:::publicAPI
        A3["forget_permits<br/><i>Shrink semaphore</i>"]:::publicAPI
        A4["add_permits<br/><i>Expand semaphore</i>"]:::publicAPI
    end

    %% Acquire flow
    A2 --> B0["Create MaybeForgetFuture"]:::internalImpl
    B0 --> B{Check to_forget > 0?}:::futureImpl
    B -->|Yes| C["Acquire permit from inner semaphore"]:::futureImpl
    B -->|No| G["Acquire permit for caller"]:::futureImpl

    subgraph "Future Implementation - Internal"
        C --> D["Call permit.forget<br/>Permanently reduce semaphore"]:::futureImpl
        D --> E["Decrement to_forget counter"]:::futureImpl
        E --> F["Create new inner future<br/>Replace old one"]:::futureImpl
        F --> B
    end

    G --> H["Return OwnedSemaphorePermit to caller"]

    %% forget_permits flow
    A3 --> J{Try to acquire<br/>immediate permits?}:::internalImpl
    J -->|Success| K["permit.forget<br/>Immediately reduce semaphore"]:::internalImpl
    J -->|Failure| L["Increment to_forget counter<br/>for later processing"]:::internalImpl

    %% add_permits flow
    A4 --> N{to_forget >= count?}:::internalImpl
    N -->|Yes| O["Decrease to_forget by count<br/>No actual permits added"]:::internalImpl
    N -->|No| P["Add actual permits to semaphore<br/>Reset to_forget counter"]:::internalImpl

```
