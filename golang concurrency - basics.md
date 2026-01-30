
## Exercise 1: Basic Channel - Send and Receive

**Concept**: Channels let goroutines communicate. The `<-` operator sends/receives data.

```go
package main

import "fmt"

func main() {
    // Create a channel that transports integers
    ch := make(chan int)
    
    // Start a goroutine (like a lightweight thread)
    go func() {
        ch <- 42  // Send 42 into the channel
    }()
    
    value := <-ch  // Receive from the channel
    fmt.Println(value)  // Prints: 42
}
```

**Key points**:
- `make(chan int)` creates a channel for integers
- `ch <- 42` sends data into channel (arrow points INTO channel)
- `value := <-ch` receives data from channel (arrow points OUT OF channel)
- **Channels block**: The receiver waits until someone sends

---

## Exercise 2: Unbuffered vs Buffered Channels

**Concept**: Unbuffered channels block until both sender and receiver are ready. Buffered channels have capacity.

```go
package main

import "fmt"

func main() {
    // UNBUFFERED - capacity 0 (default)
    unbuffered := make(chan int)
    
    // This would DEADLOCK (block forever):
    // unbuffered <- 1  // No one is receiving!
    
    // BUFFERED - capacity 3
    buffered := make(chan int, 3)
    
    // These work fine - no receiver needed yet
    buffered <- 1
    buffered <- 2
    buffered <- 3
    
    // This would block (channel is full):
    // buffered <- 4
    
    fmt.Println(<-buffered)  // 1
    fmt.Println(<-buffered)  // 2
    fmt.Println(<-buffered)  // 3
}
```

**Key points**:
- `make(chan int)` = unbuffered (blocks immediately)
- `make(chan int, 3)` = buffered with capacity 3
- Buffered channels let you send without a receiver, up to capacity

---

## Exercise 3: Channel Direction - Send-only and Receive-only

**Concept**: You can restrict channels to only send or only receive for safety.

```go
package main

import "fmt"

// This function can only SEND to the channel
func producer(ch chan<- int) {  // chan<- means "send-only"
    for i := 0; i < 5; i++ {
        ch <- i
    }
    close(ch)  // Close when done sending
}

// This function can only RECEIVE from the channel
func consumer(ch <-chan int) {  // <-chan means "receive-only"
    for value := range ch {  // range loops until channel is closed
        fmt.Println(value)
    }
}

func main() {
    ch := make(chan int, 5)
    
    go producer(ch)   // Send data
    consumer(ch)      // Receive data
}
```

**Key points**:
- `chan<- int` = send-only channel
- `<-chan int` = receive-only channel
- `close(ch)` signals no more data will be sent
- `range` on a channel receives until it's closed

---

## Exercise 4: Select Statement - Multiple Channels

**Concept**: `select` lets you wait on multiple channels at once (like a switch for channels).

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    ch1 := make(chan string)
    ch2 := make(chan string)
    
    go func() {
        time.Sleep(1 * time.Second)
        ch1 <- "from channel 1"
    }()
    
    go func() {
        time.Sleep(2 * time.Second)
        ch2 <- "from channel 2"
    }()
    
    // Wait for EITHER channel
    for i := 0; i < 2; i++ {
        select {
        case msg1 := <-ch1:
            fmt.Println(msg1)
        case msg2 := <-ch2:
            fmt.Println(msg2)
        }
    }
}
```

**Key points**:
- `select` waits on multiple channel operations
- Whichever channel is ready first, that case runs
- Like a switch statement but for channel operations

---

## Exercise 5: Select with Default - Non-blocking Operations

**Concept**: Adding a `default` case makes select non-blocking.

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    ch := make(chan int, 2)
    
    // Try to receive (non-blocking)
    select {
    case value := <-ch:
        fmt.Println("Received:", value)
    default:
        fmt.Println("No data available")  // Runs immediately
    }
    
    // Send some data
    ch <- 42
    
    // Try again
    select {
    case value := <-ch:
        fmt.Println("Received:", value)  // This runs
    default:
        fmt.Println("No data available")
    }
    
    // Non-blocking send
    ch <- 1
    ch <- 2  // Channel is now full (capacity 2)
    
    select {
    case ch <- 3:
        fmt.Println("Sent 3")
    default:
        fmt.Println("Channel full, can't send")  // This runs
    }
}
```

**Key points**:
- `default` in select makes it non-blocking
- If no channel is ready, default case runs immediately
- Useful for "try to send/receive, but don't wait"

---

## Exercise 6: Worker Pool Pattern

**Concept**: Multiple workers receive jobs from a shared channel.

```go
package main

import (
    "fmt"
    "time"
)

func worker(id int, jobs <-chan int, results chan<- int) {
    for job := range jobs {
        fmt.Printf("Worker %d processing job %d\n", id, job)
        time.Sleep(time.Second)  // Simulate work
        results <- job * 2       // Send result
    }
}

func main() {
    jobs := make(chan int, 10)
    results := make(chan int, 10)
    
    // Start 3 workers
    for w := 1; w <= 3; w++ {
        go worker(w, jobs, results)
    }
    
    // Send 9 jobs
    for j := 1; j <= 9; j++ {
        jobs <- j
    }
    close(jobs)  // No more jobs
    
    // Collect 9 results
    for r := 1; r <= 9; r++ {
        result := <-results
        fmt.Println("Result:", result)
    }
}
```

**Key points**:
- Multiple goroutines can receive from the same channel
- Channels distribute work automatically
- Classic pattern for parallel processing

---

## Exercise 7: Timeout Pattern with Select

**Concept**: Use `time.After` to implement timeouts.

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    ch := make(chan string)
    
    go func() {
        time.Sleep(3 * time.Second)
        ch <- "result"
    }()
    
    select {
    case result := <-ch:
        fmt.Println("Received:", result)
    case <-time.After(2 * time.Second):
        fmt.Println("Timeout! Took too long")
    }
}
```

**Key points**:
- `time.After(duration)` returns a channel that receives after the duration
- Use with select to implement timeouts
- Prevents waiting forever

---

## Exercise 8: Channel of Channels (Advanced)

**Concept**: Channels can transport other channels! Useful for request-response patterns.

```go
package main

import "fmt"

type Request struct {
    data     string
    response chan string  // Each request has its own response channel
}

func server(requests chan Request) {
    for req := range requests {
        // Process the request
        result := "Processed: " + req.data
        
        // Send response back through the request's own channel
        req.response <- result
    }
}

func main() {
    requests := make(chan Request)
    
    go server(requests)
    
    // Make 3 requests
    for i := 1; i <= 3; i++ {
        // Create a response channel for this request
        respChan := make(chan string)
        
        // Send request
        requests <- Request{
            data:     fmt.Sprintf("request-%d", i),
            response: respChan,
        }
        
        // Wait for THIS request's response
        response := <-respChan
        fmt.Println(response)
    }
    
    close(requests)
}
```

**Key points**:
- Channels can hold any type, including other channels
- Allows request-specific responses
- Each request gets its own "return address"

---

## Summary

1. **Basic**: Channels send/receive data between goroutines
2. **Buffered**: Channels can have capacity to store data
3. **Direction**: Restrict to send-only or receive-only
4. **Select**: Wait on multiple channels
5. **Default**: Non-blocking channel operations
6. **Worker Pool**: Multiple goroutines sharing work
7. **Timeout**: Prevent waiting forever
8. **Advanced**: Channels of channels for complex patterns

Which exercise would you like me to explain more deeply?




[[Worker pools toy]]