---
tags:
  - rust
  - long_term
  - plans
created: 2025-09-12
---
[[async cancellation safety - notes]]
## Pingora
### Thread local vs Arc Mutex
timer.rs in pingora 

a ThreadLocal allows each thread to have its own instance of a variable, which is useful when you want to avoid synchronization overhead between threads accessing their respective copies of the variable.

### Rust example-futures 
* In 500 lines. 
* https://medium.com/@ning_gt/bridging-async-and-sync-rust-code-a-lesson-learned-while-working-with-tokio-6173b1efaca4
* https://without.boats/blog/let-futures-be-futures/


### Tokio Tutorial
https://www.youtube.com/watch?v=KYL4FtKY8Mc

### Rust Async tutorials
https://www.youtube.com/watch?v=uD5hBVHwyDM
https://www.youtube.com/watch?v=fTXuGRP1ee4

Write async from ground up --- https://www.youtube.com/watch?v=7pU3gOVAeVQ&ab_channel=RustNationUK


https://github.com/thebracket/Ardan-1HourAsync/  -- Rust Async Video



# rust protochakers series 

## Mental models of rust 
https://kerkour.com/rust-mental-models

## Basics

1. Trait 
2. Enum
3. Futures


#### FUTURES 
1. Lazy Futures - https://hegdenu.net/posts/understanding-async-await-1/
2. Mutex in Futures - the case of blocking - https://hegdenu.net/posts/understanding-async-await-3/
3. Manually writing futures - https://hegdenu.net/posts/understanding-async-await-4/

### SEND + SYNC 
1. Send Bound Problem  -- What are send bounds ? Why do you need them ?  - https://smallcultfollowing.com/babysteps/blog/2023/02/01/async-trait-send-bounds-part-1-intro/
2. https://smallcultfollowing.com/babysteps/blog/2023/02/13/return-type-notation-send-bounds-part-2/
3. Send Bound Problem -- async_fn in traits -- https://broch.tech/posts/rust-async-fn-trait/
4. whether or not to use async traits. https://smallcultfollowing.com/babysteps/blog/2023/03/12/to-async-trait-or-just-to-trait/
5. Async Destructures -- 
6. https://theincredibleholk.org/blog/2023/02/16/lightweight-predictable-async-send-bounds/
7. https://theincredibleholk.org/blog/2023/02/13/inferred-async-send-bounds/
8. https://theincredibleholk.org/blog/2022/12/19/async-fn-in-trait-object-update/

### Async closures 
https://smallcultfollowing.com/babysteps/blog/2023/05/09/giving-lending-and-async-closures/

## Rust  - async with care
1. async rust is a bad language -- https://bitbashing.io/async-rust.html
2. https://www.qovery.com/blog/common-mistakes-with-rust-async
3. https://ibraheem.ca/posts/too-many-web-servers/#:~:text=I've%20found%20that%20one,designed%20the%20way%20it%20was
4. https://notgull.net/why-you-want-async
5. 
6. https://www.scylladb.com/2022/01/12/async-rust-in-practice-performance-pitfalls-profiling/

## Questions 

0. What is difference between concurrency and async programming? Are not those two things same?
	1. Models of concurrency - SHARED MEMORY and MESSAGE PASSING
	2. SHARED MEMORY
		1. same physical memory
		2. same file system 
		3. same objects shared 
	3. MESSAGE PASSING
		1. UNIX programs - pipe operator 
		2. client server architecture - erlang processes / actor model 
0. Why do you need concurrency at all? 
	1. Mix compute and I/O
	2. Applications like Nginx and Node.js are largely single-threaded (at least in terms of compute). But handle large I/O 
1. How is async in Rust different from async in JS ? == What is difference between Future and Promises? 
	1. Lazy
	2. Executor (runtime) of your choice
	3. [operation and execution can be decoupled: `say_world()` ](https://tokio.rs/tokio/tutorial/hello-tokio). Why? What's the use case? 
	4. C# Task (promises) are  eager. C# Tasks are inherently parallel capable. How? `Task` is unit of computation that can be stolen by a thread in thread pool.
2. [What is a runtime and why needed? ](https://docs.rs/tokio/1.34.0/tokio/runtime/index.html)
	1. [Pollster](https://crates.io/crates/pollster) : However, many of us are _not_ building highly concurrent web applications, but end up faced with an `async` function that we can't easily call from synchronous code. If you're in this position, then `pollster` is for you: it allows you to evaluate a future in-place without spinning up a heavyweight runtime like `tokio` or `async_std`.
3. When do you need async? Language level vs library level features.
	1. TLDR: I/O stuff. not heavy duty parallel computation. -- for **MASSIVE CONCURRENCY** . Little concurrency you can do with os threads. but not webserver / networking calls etc 
		1. We have too many variables to fit into CPU registers, and they will be saved on **the stack**. Each thread gets a separate stack. The stack is just a memory region where the thread can put local variables or even control flow data, like information about where to return from the currently executing function. -- memory issues mostly
	2. Async functions always return a promise, though not all functions that return a promise are async functions,[src](https://www.tedinski.com/2018/10/30/async-and-await.html) 
	3. Async/await just lets us write code in the manner we usually would, with the stopping points annotated with `await` keywords.
	4. So at its core, an async function is just one that has internal gaps where it might return early and then resume as events come in later.![[27a6ad34371f5a35f54c9b9b6125ab61d83a448953218402f87ef08191d44d13.png]]
	5. all I’ve described is the ability for async functions to “block” without actually blocking the thread.
4. [Why need async rust? ](https://without.boats/blog/why-async-rust/)
5. [How does async rust work? ](https://bertptrs.nl/2023/04/27/how-does-async-rust-work.html)
	1. https://developerlife.com/2022/03/12/rust-tokio/#:~:text=In%20Rust%2C%20a%20yield%20point,it%20must%20be%20marked%20async%20.
	2. Thinking about async Rust -- https://cliffle.com/blog/async-inversion/
	3. AsyncIterators -- https://blog.yoshuawuyts.com/async-iteration/ + https://without.boats/blog/async-iterator/
	4. Tokio under the hood --- https://kerkour.com/rust-async-await-what-is-a-runtime
	5. 
6. 
7. [IntoFuture](https://doc.rust-lang.org/std/future/trait.IntoFuture.html) = any `Into` means you are `moving` ownership to something else. 
	1. so `.await?` converts the object into `IntoFuture`
	2. Note in example: `Ready(value)` is returned. Where does this `Ready` come from ? 
		1. `Future` trait.
8. What makes concurrency hard? 
	1. dealing with errors, cancellation (safely) - https://greptime.com/blogs/2023-01-12-hidden-control-flow
		1. https://theincredibleholk.org/blog/2023/11/14/a-mechanism-for-async-cancellation/
		2. https://theincredibleholk.org/blog/2023/11/08/cancellation-async-state-machines/
		3. Drop - https://sabrinajewson.org/blog/async-drop
	2. dealing with returned values (interaction between caller and `worker`)
		1. ancestor vs caller vs worker -- more from youtube video of elixir
	3. data races () -- **Heisenbugs** -- hard to test and debug
		1. can't use `print` statements! coz print statements are slower than the actual code. If you remove print statements, you re-introduce the bug back! 
	4. ![[Pasted image 20231204173016.png]]
	5. {read `->` as `has a` } 
		1. async -> async is fine. :: Cancellation and error propagation?  
		2. sync -> async is fine. :: Pollster `block_on` or `async_fun_name().then()` in JS :: sequential composition of Futures
		3. async -> sync is fine. :: Ok
		4. async -> sync -> async (read more from Box, Pin and Suffering)
			1. Green threads are a leaky abstraction. Lots of system calls block. Many things like file I/O only present synchronous APIs, after all. [src](https://www.tedinski.com/2018/11/06/concurrency-models.html)
9. Composing futures
	1. https://kerkour.com/rust-async-combinators
- [**Sequential composition**](http://alexcrichton.com/futures-rs/futures/trait.Future.html#method.and_then): `f.and_then(|val| some_new_future(val))`. Gives you a future that executes the future `f`, takes the `val` it produces to build another future `some_new_future(val)`, and then executes that future.
    
- [**Mapping**](http://alexcrichton.com/futures-rs/futures/trait.Future.html#method.map): `f.map(|val| some_new_value(val))`. Gives you a future that executes the future `f` and yields the result of `some_new_value(val)`.
    
- [**Joining**](http://alexcrichton.com/futures-rs/futures/trait.Future.html#method.join): `f.join(g)`. Gives you a future that executes the futures `f` and `g` in parallel, and completes when _both_ of them are complete, returning both of their values.
    
- [**Selecting**](http://alexcrichton.com/futures-rs/futures/trait.Future.html#method.select): `f.select(g)`. Gives you a future that executes the futures `f` and `g` in parallel, and completes when _one of_ them is complete, returning its value and the other future. (Want to add a timeout to any future? Just do a `select` of that future and a timeout future!)

7. the need for Streams in futures 
	1. Let’s make things more interesting. Assume the protocol is pipelined, i.e., that the client can send additional requests on the socket before hearing back from the ones being processed. We want to actually process the requests sequentially, but there’s an opportunity for some parallelism here: we could read _and parse_ a few requests ahead, while the current request is being processed. Doing so is as easy as inserting one more combinator in the right place:

```rust
let requests = ParseStream::new(input);
let responses = requests.map(|req| service.process(req)).buffered(32); // <--
StreamWriter::new(responsesm, output)
```

The [`buffered` combinator](http://alexcrichton.com/futures-rs/futures/stream/trait.Stream.html#method.buffered) takes a stream of _futures_ and buffers it by some fixed amount. Buffering the stream means that it will eagerly pull out more than the requested number of items, and stash the resulting futures in a buffer for later processing. In this case, that means that we will read and parse up to 32 extra requests in parallel, while running `process` on the current one.



9. Models of thread based concurrency? 
	1. 50k OS threads are not really an issue on modern server hardware. While it might not be the most efficient [1], it will not perform so bad that it causes an availaiblity impact either.
	2. task (green) within an os thread -- more concurrency
10. Actor based models of concurrency - elixir / scala
	1. What makes it advantageous to work with actors?
		1. Mimics the mental model of world. I and you are actors communicating via message passing
		2. if you implement the erlang protocol / OTP requirements in rust, you could have a *rust / go / C* binary act as erlang node and cluster it with erlang application.
11. Why makes *Async Rust* hard? 
	1. Exectuor of your choice. `tokio` 
	2. Synchronise the async code, how? - Arc / Mutex for shared mutable state. Locking means runtime overhead.
13. How is lifetimes related to Async code?  Should everything be just static + send + sync? -- scoped threads ? (google)


https://lunatic.solutions/blog/rust-without-the-async-hard-part/
https://dev.to/joelnet/why-async-code-is-so-damn-confusing-and-a-how-to-make-it-easy-3441
https://www.thecodedmessage.com/posts/async-colors/

https://bitbashing.io/async-rust.html
https://notgull.net/why-you-want-async/

https://blog.logrocket.com/a-practical-guide-to-async-in-rust/

https://itnext.io/async-await-what-happens-under-the-hood-eef1de0dd881

https://itsallaboutthebit.com/async-simple/
https://smallcultfollowing.com/babysteps/blog/2019/10/26/async-fn-in-traits-are-hard/
https://liuhaohua.com/2022/04/16/asynchronous-programming.html

https://www.ncameron.org/blog/what-is-an-async-runtime/

https://rust-lang.github.io/async-book/01_getting_started/03_state_of_async_rust.html

https://www.tedinski.com/2018/11/13/function-coloring.html
https://www.tedinski.com/2018/10/30/async-and-await.html
https://itsallaboutthebit.com/async-simple/
https://hassamuddin.com/blog/rust-mqtt/overview/

https://smallcultfollowing.com/babysteps/blog/2022/06/13/async-cancellation-a-case-study-of-pub-sub-in-mini-redis/

https://tokio.rs/tokio/tutorial/spawning

https://build-your-own.org/blog/20230127_byor/


### Misc Rust 

1. Errors in rust 
	1. https://www.shakacode.com/blog/thiserror-anyhow-or-how-i-handle-errors-in-rust-apps/
	2. https://antoinerr.github.io/blog-website/2023/01/28/rust-anyhow.html
	3. https://nick.groenen.me/posts/rust-error-handling/



Other Rust books: 
1. https://www.amazon.com/Rust-Depth-Guidebook-Comprehensive-Exploration-ebook/dp/B0CKL3G8BQ/ref=pd_sim_d_sccl_1_5/133-4171780-8763028?pd_rd_w=gnfU4&content-id=amzn1.sym.827183ca-bff0-4aff-8800-c5c403e2994f&pf_rd_p=827183ca-bff0-4aff-8800-c5c403e2994f&pf_rd_r=5DNVYNWMHFWV6J7P0T5N&pd_rd_wg=ABsRs&pd_rd_r=f6b4b738-01cc-4bf8-b3ea-8ffbc0f4fbcf&pd_rd_i=B0CKL3G8BQ&psc=1
2. 


Hey folks,

I am gathering feedback for a book (100 pages) on async rust.

250 * 100 

Preface : Who is this book for? Why should you read it ?

1. The basics
	1. Two model of concurrent programming
		1. shared memory 
		2. message passing
	2. Concurrency vs parallelism
2. What is async? 
	2. What is blocking? 
		1. A unit of time / work - tokio::yield_now
	3. What is a runtime? 
		1. runtime = executor + reactor 
		2. types of executors in a runtime
		3. Task , Future Trait and executor
	4. Rust's Futures - difference with JS
		1. Let futures be futures 
	5. Shared state concurrency
		1. Lock based
		2. Await free / lock free concurrency
	6. Message passing via channels 

3. State of Async Ecosystem in Rust today
	1. async in std lib 
	2. Runtimes - tokio 
	3. concurrency and related crates
		1. async recursion
		2. OnceCell / RefCell / Cell / Atomics
		3. async functions in traits 
		4. timed futures - Tokio::select! 
	4. structured concurrency - What why how? - Cancellation - where Drop semantics don't work.
	5. Conclusion 
		1. Enough to get going and build! 
		2. Looking forward - what to expect from future versions of rust? 

4. Patterns of async code - with examples
	1. 
	2. Deep Dive mini redis - by Tokio team 
	3. Async cancellation / AsyncIterators / AsyncStreams
	4. Async in embedded context? - Thread locals ? 
	5. Function Coloring 
		1. calling Sync code from within async code
			1. Especially , FFI (with c libraries which are Sync)
	6. Tokio Asyncifies std library ? 
5. Errors, recovery and testing Async code

8. Threads , Parallelism and runtime (tokio)
	1. Message passing via channels
	2. Mixing os threads and tokio workers via channels
	3. rayon crate
9. Pinning - what, why , how, when?
10. Efficient data structures 
	1. A short intro to memcpy / mmap 
	2. zero copy 
11. Observability in async Rust 
	1. Tracing crate and order of execution
12. Onwards and upwards 
	1. Next steps
	2. more resources , sample projects , ideas ...




----

## Book Title:

Gentle Introduction to Async Rust

## Book Subtitle (include keywords that aren't in the title):

Async for people whose don't do rust daily

## Author Name(s):

Abhishek Tripathi. (there may be 1 more.)

## Page Count (your best estimate - allow 300 words to a page):

~100 - 120 

## Overview

The book aims to cover async parts of rust which are 'hard' to reason about. The contents of the books refresh the take on async rust by introducing the ecosystem as a whole to enable getting-started rather easily. The audience should have some prior experience with async from JS / python other ecosystems. They should be ready to do the side-reading as and when the need arises. (for example, what is dns)

A good book which is a gentle introduction and ecosystem survey would go long way in boosting rust adoption.

I recently wrote a tiny application that uses few of above concepts. Figuring out details was a little hard and confusing given so much text one has to go through. A structured primer to async rust would be nice to consolidate all those readings for me and for ecosystem in general. 

While I don't have much production experience in Rust, I am up for building small examples or a take a small project to illustrate the concepts in this book. Another idea I think that could work is taking open source libraries like iggy.rs or callysto to illustrate the concepts. I am undecided on the approach yet. 
In an ideal scenario, my preference would be taking a real world project - like a backend service for XX use case. 



## Outline


Preface : Who is this book for? Why should you read it ?

1. The basics
	1. Two model of concurrent programming
		1. shared memory 
		2. message passing
	2. Concurrency vs parallelism
2. What is async? 
	1. What is blocking? 
		1. A unit of time / work - tokio::yield_now
	2. What is a runtime? 
		1. runtime = executor + reactor 
		2. types of executors in a runtime
		3. Task , Future Trait and executor
	3. Rust's Futures - difference with JS
		1. Let futures be futures 
	4. Shared state concurrency
		1. Lock based
		2. Await free / lock free concurrency
	5. Message passing via channels 

3. State of Async Ecosystem in Rust today
	1. async in std lib
	2. Runtimes 
	3. concurrency and related crates
		1. async recursion
		2. OnceCell / RefCell / Cell / Atomics
		3. async functions in traits 
		4. timed futures - Tokio::select! 
	4. structured concurrency - What why how?
	5. Conclusion 
		1. Enough to get going and build! 
		2. Looking forward - what to expect from future versions of rust? 

4. Patterns of async code - with examples
	1. 
	2. Deep Dive mini redis - by Tokio team 
	3. Async cancellation / AsyncIterators / AsyncStreams
	4. Async in embedded context? - Thread locals ? 
	5. Function Coloring 
		1. calling Sync code from within async code
			1. Especially , FFI (with c libraries which are Sync)
	6. Tokio Asyncifies std library ? 
5. Errors, recovery and testing Async code
6. Threads , Parallelism and runtime (tokio)
	1. Message passing via channels
	2. Mixing os threads and tokio workers via channels
	3. rayon crate
7. Pinning - what, why , how, when?
8. Efficient data structures 
	1. A short intro to memcpy / mmap 
	2. zero copy 
9. Observability in async Rust 
	1. Tracing crate and order of execution
10. Onwards and upwards 
	1. Next steps
	2. more resources , sample projects , ideas ...




## Bio

Abhishek is a passionate software developer from India. He is Elixir-ist by the day and Rustacean by the night. He has built a couple of small projects in rust and is hooked at its zero-cost abstractions , cross compilation and excellent tooling. He has presented the topic of Async Rust at Rust Delhi (Feb 2024). He tweets about his findings on twitter at @twistin456. His code amusing can be found at https://github.com/twistingTwists. 

## Competing Books

Competing books  and future competitors
1. Future : Jon mentioned that he might be working on async book in rust. Owing to his experience, that'd be a deep dive aimed at intermediate rust programmers. Mine is a gentle overview for people who don't do rust daily.
2. Present: Async Book does cover basics. However it lacks information around ecosystem, crates , and full blown examples. I aim to include that in my book Async Book is also incomplete at the moment. 
3. Present: Async Rust by Maxwell Flitton, Caroline Morton : from Oreilly . Maxwell has written books about Rust previously. So he is well known within the community. 
4.  [Asynchronous Programming in Rust ](https://github.com/PacktPublishing/Asynchronous-Programming-in-Rust)by [Carl Fredrik Samson](https://www.amazon.in/Carl-Fredrik-Samson/e/B0CSLRJ9ZZ/ref=dp_byline_cont_ebooks_1) : This is a very recent work and in progress now. 
5. https://book.async.rs/ : 
6. Present: [Async Mechanics in Rust ]( https://www.amazon.com/Async-mechanics-Rust-concurrency-ecosystem-ebook/dp/B0CTCTM4L4/): 

List the books that compete indirectly or directly and tell us how yours is different and better. Why would a reader choose your book over the others that are on the market?

## PragProg Books

List any of our existing books that relate to yours. Which books might a reader need to look at first or might they be able to use after they've read yours?

## Market Size

Tell us your opinion on the market for your book. Include metrics like the number of starred projects on GitHub, number of members in related online communities, number of conference talks on this topic, etc.

## Promotional Ideas

We have a marketing team who will help promote your book. What inside info can you give them? Think webinars, courses, conference talks, blogs, meet-up groups, anything you can participate in to promote your book. What ideas do you have?

## Writing Sample(s)

We would love to see an example of your writing. If you've written any other books, please link to them here and, if possible, send us a sample chapter from somewhere in the middle of your most recent book as a separate attachment. If you've never written a book but you have written online tutorials, blog posts, or anything else designed to teach people how to do something technical, please point us to where we can see those writing samples ourselves. And if you've never done any sort of public writing before, that's okay. We may also ask you to send us a five-to-seven-page example draft from the book you'd like to publish with us after we've reviewed your proposal, but we'll provide more details on that when the time comes.