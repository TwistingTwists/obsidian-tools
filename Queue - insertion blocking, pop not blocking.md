Here's how to implement it using Go channels:

```go
package main

import (
    "errors"
    "fmt"
    "time"
)

var ErrQueueEmpty = errors.New("queue is empty")

type BlockingQueue struct {
    ch chan interface{}
}

func NewBlockingQueue(capacity int) *BlockingQueue {
    return &BlockingQueue{
        ch: make(chan interface{}, capacity),
    }
}

// Push blocks if the queue is full
func (q *BlockingQueue) Push(item interface{}) {
    q.ch <- item // Blocks when channel is full
}

// Pop is non-blocking - returns immediately if queue is empty
func (q *BlockingQueue) Pop() (interface{}, error) {
    select {
    case item := <-q.ch:
        return item, nil
    default:
        return nil, ErrQueueEmpty
    }
}

// Size returns current queue size
func (q *BlockingQueue) Size() int {
    return len(q.ch)
}

// Close closes the queue
func (q *BlockingQueue) Close() {
    close(q.ch)
}
```

**Usage example:**

```go
func main() {
    q := NewBlockingQueue(5)
    
    // Producer goroutine - Push blocks when full
    go func() {
        for i := 0; i < 10; i++ {
            fmt.Printf("Pushing %d...\n", i)
            q.Push(i) // Blocks if queue is full
            fmt.Printf("Pushed %d\n", i)
        }
        q.Close()
    }()
    
    // Consumer goroutine - Pop returns immediately
    go func() {
        for {
            time.Sleep(500 * time.Millisecond)
            item, err := q.Pop() // Non-blocking
            if err != nil {
                fmt.Printf("Pop failed: %v\n", err)
            } else {
                fmt.Printf("Popped: %v\n", item)
            }
        }
    }()
    
    time.Sleep(10 * time.Second)
}
```

**How it works:**

1. **Blocking Push**: `q.ch <- item` blocks when the buffered channel is full, naturally providing the blocking behavior you want

2. **Non-blocking Pop**: The `select` statement with a `default` case makes it non-blocking:
   - If there's an item available, it's received immediately
   - If the channel is empty, the `default` case executes and returns an error

3. **Capacity**: The buffered channel size controls the queue capacity

This channel-based approach is much simpler and more idiomatic in Go! The channel handles all the synchronization internally, so you don't need explicit mutexes or condition variables.

**Bonus - Pop with context/timeout:**

If you want more control, you can add a Pop with timeout:

```go
func (q *BlockingQueue) PopWithTimeout(timeout time.Duration) (interface{}, error) {
    select {
    case item := <-q.ch:
        return item, nil
    case <-time.After(timeout):
        return nil, errors.New("timeout")
    }
}
```