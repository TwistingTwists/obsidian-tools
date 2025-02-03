---
tags:
  - rust
  - docs
---
# Example 1


> **Note:** This example uses the `tokio` runtime and the `futures-util` crate. Make sure to add the following dependencies to your `Cargo.toml`:

```toml
[dependencies]
tokio = { version = "1", features = ["full"] }
futures-util = "0.3"
```

---

```rust
// main.rs

use std::pin::Pin;
use std::task::{Context, Poll};

use futures_util::{stream::BoxStream, Stream, StreamExt};

/// A simple enum representing a status, used here for error handling.
#[derive(Debug)]
pub enum Status {
    Error(String),
}

/// Our custom asynchronous message stream wrapper.
/// It wraps any stream that produces `Result<T, Status>` items.
pub struct Streaming<T>(BoxStream<'static, Result<T, Status>>);

impl<T> Streaming<T> {
    /// Create a new Streaming instance from any stream that yields `Result<T, Status>`.
    ///
    /// The provided stream must be `Send` and have a `'static` lifetime.
    #[inline]
    pub fn new<S>(stream: S) -> Self
    where
        S: Stream<Item = Result<T, Status>> + Send + 'static,
    {
        Self(stream.boxed())
    }
}

impl<T> Stream for Streaming<T> {
    type Item = Result<T, Status>;

    /// Polls the underlying stream for the next item.
    #[inline]
    fn poll_next(mut self: Pin<&mut Self>, cx: &mut Context<'_>) -> Poll<Option<Self::Item>> {
        // Delegate the polling to the inner boxed stream.
        self.0.poll_next_unpin(cx)
    }
}

/// A simple message type for demonstration purposes.
#[derive(Debug)]
struct Message(i32);

#[tokio::main]
async fn main() {
    // Create a vector of messages wrapped in `Result`.
    // One of the messages simulates an error.
    let messages = vec![
        Ok(Message(1)),
        Ok(Message(2)),
        Err(Status::Error("An error occurred".into())),
        Ok(Message(3)),
    ];

    // Create a stream from the vector.
    let message_stream = futures_util::stream::iter(messages);

    // Wrap the stream using our custom Streaming type.
    let mut streaming = Streaming::new(message_stream);

    // Process the stream asynchronously.
    println!("Starting stream processing:");
    while let Some(result) = streaming.next().await {
        match result {
            Ok(msg) => println!("Received: {:?}", msg),
            Err(err) => eprintln!("Error: {:?}", err),
        }
    }
    println!("Stream processing complete.");
}
```

---

### Explanation

1. **Defining `Status` and `Message`:**
    
    - `Status` is an enum that, in this example, only has one variant (`Error`), representing an error with a message.
    - `Message` is a simple struct wrapping an integer, used to represent successful messages.
2. **The `Streaming` Struct:**
    
    - `Streaming<T>` is a tuple struct that wraps a `BoxStream` of items of type `Result<T, Status>`.
    - The `new` method accepts any stream that yields `Result<T, Status>`, boxes it, and stores it in the struct.
3. **Implementing the `Stream` Trait:**
    
    - The `poll_next` method delegates to the inner stream using `poll_next_unpin`, making it compatible with asynchronous code.
4. **The `main` Function:**
    
    - A vector of messages (with one error) is turned into a stream.
    - This stream is wrapped in the `Streaming` type.
    - The stream is processed asynchronously using a loop that awaits each item and prints it out.

Compile and run this project to see the output, which demonstrates how the custom streaming wrapper processes both successful messages and errors.


# Custom Streaming implementaiton
src: https://github.com/poem-web/poem/blob/85741b27581205d8c623bbb407d4d1908b1475dc/poem-grpc/src/streaming.rs


```rust
use std::{
    pin::Pin,
    task::{Context, Poll},
};

use futures_util::{stream::BoxStream, Stream, StreamExt};

use crate::Status;

/// Message stream
pub struct Streaming<T>(BoxStream<'static, Result<T, Status>>);

impl<T> Streaming<T> {
    /// Create a message stream
    #[inline]
    pub fn new<S>(stream: S) -> Self
    where
        S: Stream<Item = Result<T, Status>> + Send + 'static,
    {
        Self(stream.boxed())
    }
}

impl<T> Stream for Streaming<T> {
    type Item = Result<T, Status>;

    #[inline]
    fn poll_next(mut self: Pin<&mut Self>, cx: &mut Context<'_>) -> Poll<Option<Self::Item>> {
        self.0.poll_next_unpin(cx)
    }
}
```

# tokio::pin 

```rust
use std::pin::Pin;
use futures::future::Future;
use futures::stream::{self, Stream, StreamExt};

/// An async function that returns a computed value.
async fn compute_value() -> i32 {
    // Simulate some async work.
    1 + 2
}

/// An async stream that yields several numbers.
fn number_stream() -> impl Stream<Item = i32> {
    // Create a stream that yields numbers 0..5.
    stream::iter(0..5)
}

#[tokio::main]
async fn main() {
    // --- Example 1: Using tokio::pin! (stack pinning) for a Future ---
    // Create a future that computes a value.
    let mut fut = compute_value();
    // Use tokio::pin! to pin `fut` on the stack.
    tokio::pin!(fut);
    // Now `fut` is a `Pin<&mut _>` and we can await it.
    let result = fut.await;
    println!("Stack-pinned future result: {}", result);

    // --- Example 2: Using Box::pin (heap pinning) for a Future ---
    // Box::pin allocates the future on the heap and pins it.
    let heap_fut: Pin<Box<dyn Future<Output = i32>>> = Box::pin(compute_value());
    let result_heap = heap_fut.await;
    println!("Heap-pinned future result: {}", result_heap);

    // --- Example 3: Using Box::pin to pin a Stream ---
    // Create a stream that yields numbers.
    let my_stream = number_stream();
    // Box::pin pins the stream on the heap, making its type easier to hide behind a trait object.
    let mut pinned_stream: Pin<Box<dyn Stream<Item = i32> + Send>> = Box::pin(my_stream);

    println!("Stream outputs:");
    while let Some(val) = pinned_stream.next().await {
        println!("  {}", val);
    }
}

```