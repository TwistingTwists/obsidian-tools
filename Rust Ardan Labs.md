---
tags:
  - rust
  - course
  - concurrency
---
## 1.12 serialsation 

> while true and loop 

```rust

fn main() {
    println!("Hello, world!");
    
    let mut i = 0;
//  let value =   loop {
//     i = i + 1 ;
//         if i > 10{
//             break i;
//         }
//     };
    
    
    let value =   while true {
    i = i + 1 ;
        if i > 10{
            break i;
        }
    };
    println!("{value:?}");
}

```





```rust
fn main() {
    'outer: for x in 1..5 {
        println!("x: {x}");
        let mut i = 0;
        while i < x {
            println!("x: {x}, i: {i}");
            i += 1;
            if i == 3 {
                break 'outer;
            }
        }
    }
}


```
`if i == 3 { break 'outer; }`: This condition checks if `i` is equal to 3. If true, it breaks out of the loop labeled `'outer`. This means it exits both the `while` loop and the `for` loop labeled `'outer`.


```rust

// you can swap runtime values by swapping references

fn main() {
    let a = 10;
    let b = 2;
    
    let mut x_coord: &i32 = &a;
    let mut y_coord: &i32 = &b;
    
    println!("{},{}", a, b);
    println!("{},{}", x_coord, y_coord);
    
    let mut temp = x_coord;
    // changing x_coord does not change the value it holds. (unlike C++ )
    x_coord = y_coord;
    y_coord = temp;
    
    println!("{},{}", x_coord, y_coord);
    println!("{},{}", a, b);

}
```

## 2. Threads 

* handle and join , move closure (so that the ownership of all the variables is taken by thread)
* Scoped threads
* Atomic and Lazy Initialisation
* Mutex
* RwLock 



Reading list 

https://docs.rs/tokio-scoped/latest/tokio_scoped/
https://greptime.com/blogs/2023-03-09-bridging-async-and-sync-rust
https://thenewstack.io/using-rustlangs-async-tokio-runtime-for-cpu-bound-tasks/
https://github.com/andybarron/tokio-rayon
https://rustmagazine.org/issue-4/how-tokio-schedule-tasks/
https://raindev.io/blog/mixing-sync-and-async-rust/
https://docs.rs/tokio/latest/tokio/sync/mpsc/index.html#communicating-between-sync-and-async-code


### Crust of Rust - Pointers

- Basic types of interior mutability via 
Cell, RefCell,  Mutex (under sync module)
	- Box does not provide interior mutability => if you have ref to a `Box<T>` you cannot modify the value inside 
	- can't get a pointer into the `Cell`. Modification / getting the value is possible with `Cell` api. But not the pointer to inside value.
	- `Cell` doesn't impl `Sync` => can't give away ref to Cell (`Sync`) across threads
		- Single threaded env => only one pointer to Cell is in action => Can  mutate value inside. Since, no one has pointer to the value inside `Cell`
	- `Cell` = impl only for small `Copy` types
		- Cell is implemented via `UnsafeCell` which gives out the raw pointer to the value inside. `UnsafeCell` is the primitive by which the interior mutability is implemented.


### Crust of Rust - Dispatch

- Monomorphisation (over Generics) 
	- giving away rust library as binary? (problem if you have 'generic Traits' in the library)
		- not ideal because monomorphization . Hence if a user has a New Type , he can't use the given binary. Rust has to include the lib user's impl in final code.
		- So, if you want to give away dynamic library as cdylib => cannot have generics :\ 
	- Other costs
		- Compile time generation for each type
		- Runtime - can't put ONE function in instruction cache of CPU. Because many 'versions' - monomorphized exist! 
- Dispatch - Static vs Dynamic 
	- `pub fn strlen(s: impl Hei)` - static dispatch => get monomorphized to `strlen_str(s: &str)` if `impl Hei for &str `  exists
	- `pub fn strlen_dyn(s: &dyn Hei)` - pointer indirection on stack.
	- `pub fn strlen_dyn(s: Box<dyn Hei>)` - pointer indirection for object on heap


### Crust of Rust - Tokio 

- LocalSet 
	- What 
	- When do you need it? - Run not Send Futures on tokio
	- You can have description of the task / future as `Send` but have the execution not `Send`

- Tokio Mutex vs Sync Mutex
	- tokio lock guard is dropped when other future is called? - NO
		- lock_guard is dropped => lock is released. So, lock_guard should NOT be released when the task is put to sleep and other is running
	- spawn_blocking - happens on a separate thread
	- no task priorities in tokio :\ 
	- no NUMA aware scheduler in Tokio 
		- on high core machines, work stealing across CPU cores might be super costly. NUMA awareness
- tokio console

- Resources in Tokio 
	- they don't contain any futures. They are leaf future-compatible stuff
	- so, in most cases, when a future is Pending => Leaf future (future-compatible resouce) is pending.
	- `task::Context`  in tokio - has `waker.wake()`
		- when task is pending (most likely due to a resource) => moved to "non-runnable" queue
		- __somene__ calls `wake` to tell tokio to put the task in "runnable" queue
			- someone = IO event loop
		- scheduler = chooses what to pick up next from "runnable" queue => calls `poll`
		- IO event loop = lookin for events (has any event happened for all the tasks for which I have `waker` for)
		- 

| topic         | layman                                                              |
| ------------- | ------------------------------------------------------------------- |
| lifetimes     | pointer to the value and the value itself should not go out of sync |
| smart pointer | - where the data is: stack vs heap<br>-                             |
|               |                                                                     |
