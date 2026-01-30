
# create blocking queue from scratch 

```go
package main

import "fmt"

// Define the struct type
type BlockingQueue struct {
    queue chan interface{}  // A channel that can hold any type
}

// Constructor function - returns a pointer
func NewBlockingQueue(capacity int) *BlockingQueue {
    return &BlockingQueue{  // & creates a pointer to the new BlockingQueue
        queue: make(chan interface{}, capacity),
    }
}

// Method with receiver (bq *BlockingQueue)
// The * means bq is a pointer to BlockingQueue
func (bq *BlockingQueue) Put(item interface{}) {
    bq.queue <- item  // Send item to the channel
}

// Another method - Take receives from the channel
func (bq *BlockingQueue) Take() interface{} {
    return <-bq.queue  // Receive from the channel
}

func main() {
    bq := NewBlockingQueue(5)  // bq is a pointer to BlockingQueue
    bq.Put(100)                // Call method on the pointer
    value := bq.Take()         // Get value back
    fmt.Println(value)         // Prints: 100
}
```