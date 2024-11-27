---
tags:
  - rust
  - blogs
---


### Rust 

ONNX Runtime in Rust




## Hot or Not interview  - 2024-04-26

###### Q1

```rust
fn main() {

let x: u8 = 1;

const K: u8 = 2;

macro_rules! m {

() => {

print!("{}{}", x, K);

};

}

{

let x: u8 = 3;

const K: u8 = 4;

m!();

}

}
// 14
```

##### Q2
```rust
fn main() {

let (.., x, y) = (0, 1, ..);

print!("{}", b"066"[y][x]);

}
```


##### Q3 
```rust
fn main() {

let mut a = 5;

let mut b = 3;

print!("{}", a-- -  --b);

}
```


##### Q4
```rust
struct Guard;

impl Drop for Guard {

fn drop(&mut self) {

print!("1");

}

}

fn main() {

let _guard = Guard;

print!("3"); // 3

  

let _ = Guard;

  

// _ -> Drop // 1

  

print!("2"); // 2

  

// _guard goes out of scope -> Drop // 1

}

// 3121
```

##### Q5
```rust
trait Trait {

fn f(&self);

fn g(&self);

}

  

struct S;

impl S {

fn f(&self) {

print!("1");

}

fn g(&mut self) {

print!("1");

}

}

impl Trait for S {

fn f(&self) {

print!("2");

}

fn g(&self) {

print!("2");

}

}

fn main() {

S.f(); // 1 S.g(); // 2

  

// If there is a trait on S and S => which one is preferred? (auto)

}
```


## Async Rust Book 



## Effective rust
https://www.lurklurk.org/effective-rust/

### Rust and python 

* Mainmatter blog -- using rust and python with **pyo3**

## gpui2 by zed 

https://github.com/zed-industries/zed/blob/main/crates/gpui/examples/hello_world.rs


## Adding wasm support for rust project 

https://github.com/tailcallhq/tailcall/pull/1039/files


## Lifetimes in Rust
https://blog.vashishtha.in/temporary-lifetimes/


## Ugly Syntax rust
https://matklad.github.io/2023/01/26/rusts-ugly-syntax.html#Rust-s-Ugly-Syntax


### firecracker internals
https://www.talhoffman.com/2021/07/18/firecracker-internals/


## actor framework in rust
https://actix.rs/docs/actix/getting-started
https://mainmatter.com/blog/2018/06/11/actix/
https://doc.rust-lang.org/book/ch16-00-concurrency.html
https://rust-lang.github.io/async-book/02_execution/02_future.html


## deploying rust and distributing
https://dzfrias.dev/blog/deploy-rust-cross-platform-github-actions

## async runtime
https://www.ncameron.org/blog/what-is-an-async-runtime/
https://www.youtube.com/watch?v=XZtlD_m59sM
https://www.youtube.com/watch?v=0HwrZp9CBD4

## futures
https://docs.rs/futures/latest/futures/#:~:text=Futures%20are%20single%20eventual%20values,for%20asynchronous%20writing%20of%20data.
https://www.viget.com/articles/understanding-futures-in-rust-part-1/


https://rust-unofficial.github.io/patterns/idioms/coercion-arguments.html


### elixir stuff in rust
https://github.com/brndnmtthws/genserver/blob/main/src/lib.rs
https://github.com/bastion-rs/bastion


###  rust starter projects to look at n make notes
https://practice.rs/elegant-code-base.html

https://github.com/hrkfdn/ncspot
https://github.com/kyclark/command-line-rust
https://github.com/tokio-rs/mini-redis


## Async rust
https://hegdenu.net/posts/understanding-async-await-1/
https://jmmv.dev/2023/06/iii-iv-task-queue.html
https://bcnrust.github.io/devbcn-workshop/

https://www.informatik.uni-bremen.de/agbkb/lehre/programmiersprachen/artikel/EWD-notes-structured.pdf
https://rust-lang.github.io/async-book/
https://blog.yoshuawuyts.com/tree-structured-concurrency/


## Rust Advanced Trait + Option
https://apollolabsblog.hashnode.dev/unlocking-possibilities-4-reasons-why-esp32-and-rust-make-a-winning-combination
https://www.youtube.com/watch?v=HA_e1c0HbgQ&ab_channel=danlogs
https://www.youtube.com/watch?v=6c7pZYP_iIE&ab_channel=LoganSmith
https://betterprogramming.pub/how-to-wrap-your-errors-with-enums-when-using-error-stack-77b122016e6e





### Notes from Spoti-dl 
https://github.com/dhruv-ahuja/spoti-dl/blob/home/Cargo.toml

## Notes from ripgrep-all

1.
