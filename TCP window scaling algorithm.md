---
tags:
  - projects
---
```rust
use reqwest::Client;
use std::time::Duration;
use tokio::time::sleep;

#[tokio::main]
async fn main() {
    let client = Client::builder()
        .pool_idle_timeout(Duration::from_secs(600)) // High upper bound
        .build()
        .unwrap();

    let url = "https://example.com";

    // Initial request to establish connection
    let resp = client.get(url).send().await.unwrap();
    println!("Initial request: {}", resp.status());

    // Probing parameters
    let mut lower = Duration::from_secs(1);
    let mut upper = Duration::from_secs(600);
    let mut step = lower;

    loop {
        println!("Waiting for {:?} before retrying...", step);
        sleep(step).await;

        let result = client.get(url).send().await;

        match result {
            Ok(resp) => {
                println!("✅ Connection still alive at {:?} (Status: {})", step, resp.status());
                lower = step;
                step = if step * 2 < upper {
                    step * 2
                } else {
                    (lower + upper) / 2
                };
            }
            Err(e) => {
                println!("❌ Connection closed after {:?}: {}", step, e);
                upper = step;
                step = (lower + upper) / 2;
            }
        }

        // Stop if the bounds converge closely
        if upper - lower <= Duration::from_secs(5) {
            println!(
                "\n🎯 Estimated idle timeout: ~{} seconds",
                lower.as_secs()
            );
            break;
        }
    }
}

```

```bash
Initial request: 200 OK
Waiting for 1s...
✅ Connection still alive at 1s
Waiting for 2s...
✅ Connection still alive at 2s
...
❌ Connection closed after 120s: connection error
🎯 Estimated idle timeout: ~115 seconds

```


-----


## 🔁 HTTP/1.1 Keep-Alive vs HTTP/2 Connection Persistence

While both HTTP/1.1 and HTTP/2 **reuse connections** to reduce overhead (i.e. keep the TCP connection open across multiple requests), they do it in **very different ways**.

Let’s break it down:

---

### 🧱 HTTP/1.1 Keep-Alive

- **One request per connection at a time**
    
- Supports _Connection: keep-alive_ header (default in HTTP/1.1)
    
- Connections are reused **sequentially**: wait for response before reusing the connection.
    
- Keep-alive is controlled via:
    
    - Client and server `Connection: keep-alive` headers
        
    - Server may specify `Keep-Alive: timeout=5, max=100` (hints for idle timeout & request limit)
        
    - After idle timeout or max requests, the server may close the connection
        

#### ⚠️ Downsides

- No parallelism over the same TCP connection
    
- Head-of-line blocking: if the first request is slow, the others wait
    
- Multiple TCP connections are opened for concurrency
    

---

### 🚄 HTTP/2

- Uses **a single TCP connection** per origin for **many parallel requests**
    
- Introduces **multiplexing**: multiple streams (requests/responses) can happen concurrently over one connection
    
- No `Connection: keep-alive` headers — persistence is assumed
    
- Stream management, flow control, and ping frames help detect idle/disconnected connections
    
- Idle connection timeout is usually managed by the server, load balancer, or intermediary (e.g. ALB, NGINX)
    

#### ✅ Benefits

- Multiplexing = much better for concurrent requests
    
- Less latency (no TCP slow start for each connection)
    
- More efficient for mobile networks and high latency links
    
- Persistent by default unless closed explicitly
    

---

### 🧪 Key Differences

|Feature|HTTP/1.1|HTTP/2|
|---|---|---|
|Multiplexing|❌ One request at a time|✅ Many parallel streams|
|Keep-Alive mechanism|`Connection: keep-alive` header|Persistent by default, no header needed|
|Connection reuse|Sequential|Parallel|
|Idle timeout control|Via headers / server config|Server config or protocol-level pings|
|TCP connections per host|Often many|Usually one|
|Head-of-line blocking|❌ Yes|✅ Solved (per-stream isolation)|

---

### 💡 TL;DR

- **HTTP/1.1 keep-alive** is like asking the server "hey, can I keep using this socket?"
    
- **HTTP/2** assumes the socket stays alive, and uses **streams** to do all the work in parallel — much faster and smarter.
    

---

Want to see how a particular server handles HTTP/2? Tools like `curl --http2 -v` or `h2c` are great for exploring.