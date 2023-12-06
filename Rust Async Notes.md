
### Tokio Tutorial


## Rust Async Book 



## Basics

1. Trait 
	1. 
2. Enum
3. Futures

## Questions 

0. What is difference between concurrency and async programming? Are not those two things same?
	1. Models of concurrency - SHARED MEMORY and MESSAGE PASSING
	2. SHARED MEMORY - 
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
2. [What is a runtime and why needed? ](https://docs.rs/tokio/1.34.0/tokio/runtime/index.html)
	1. [Pollster](https://crates.io/crates/pollster) : However, many of us are _not_ building highly concurrent web applications, but end up faced with an `async` function that we can't easily call from synchronous code. If you're in this position, then `pollster` is for you: it allows you to evaluate a future in-place without spinning up a heavyweight runtime like `tokio` or `async_std`.
3. When do you need async? 
	1. TLDR: I/O stuff. not heavy duty parallel computation. -- for **MASSIVE CONCURRENCY** . Little concurrency you can do with os threads. but not webserver / networking calls etc 
		1. We have too many variables to fit into CPU registers, and they will be saved on **the stack**. Each thread gets a separate stack. The stack is just a memory region where the thread can put local variables or even control flow data, like information about where to return from the currently executing function. -- memory issues mostly
4. [IntoFuture](https://doc.rust-lang.org/std/future/trait.IntoFuture.html) = any `Into` means you are `moving` ownership to something else. 
	1. so `.await?` converts the object into `IntoFuture`
	2. Note in example: `Ready(value)` is returned. Where does this `Ready` come from ? 
		1. `Future` trait.
5. What makes concurrency hard? 
	1. dealing with errors , cancellation (safely)
	2. dealing with returned values (interaction between caller and `worker`)
		1. ancestor vs caller vs worker -- more from youtube video of elixir
	3. data races () -- **Heisenbugs** -- hard to test and debug
		1. can't use `print` statements! coz print statements are slower than the actual code. If you remove print statements, you re-introduce the bug back! 
	4. Async functions always return a promise, though not all functions that return a promise are async functions,[src](https://www.tedinski.com/2018/10/30/async-and-await.html) 
	5. Async/await just lets us write code in the manner we usually would, with the stopping points annotated with `await` keywords.
	6. So at its core, an async function is just one that has internal gaps where it might return early and then resume as events come in later.![[27a6ad34371f5a35f54c9b9b6125ab61d83a448953218402f87ef08191d44d13.png]]
	7. ![[Pasted image 20231204173016.png]]
	8. {read `->` as `has a` } 
		1. async -> async is fine. :: Cancellation and error propagation?  
		2. sync -> async is fine. :: Pollster `block_on`
		3. async -> sync is fine. :: Ok
		4. async -> sync -> async (read more from Box, Pin and Suffering)
			1. Green threads are a leaky abstraction. Lots of system calls block. Many things like file I/O only present synchronous APIs, after all. [src](https://www.tedinski.com/2018/11/06/concurrency-models.html)
6. Models of thread based concurrency? 
	1. 50k OS threads are not really an issue on modern server hardware. While it might not be the most efficient [1], it will not perform so bad that it causes an availaiblity impact either.
	2. task (green) within an os thread -- more concurrency
7. Actor based models of concurrency - elixir / scala
	1. What makes it advantageous to work with actors?
		1. Mimics the mental model of world. I and you are actors communicating via message passing
		2. if you implement the erlang protocol / OTP requirements in rust, you could have a *rust / go / C* binary act as erlang node and cluster it with erlang application.
8. 
9. 
10. Why makes *Async Rust* hard? 
	1. Exectuor of your choice. `tokio` 
	2. 
11. Box Pin and suffering 




### Misc Rust 

1. Errors in rust 
	1. https://www.shakacode.com/blog/thiserror-anyhow-or-how-i-handle-errors-in-rust-apps/
	2. https://antoinerr.github.io/blog-website/2023/01/28/rust-anyhow.html
	3. https://nick.groenen.me/posts/rust-error-handling/
2. 