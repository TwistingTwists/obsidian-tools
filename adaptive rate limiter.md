
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


