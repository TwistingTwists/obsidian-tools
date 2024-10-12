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




### Crust of Rust - Dispatch

- Monomorphisation (over Generics) 
	- giving away rust library as binary?
		- not ideal because monomorphization . Hence if a user has a New Type , he can't use the given binary. Rust has to include the lib user's impl in final code.
		- So, if you want to give away dynamic library as cdylib => cannot have generics :\ 
	- Other costs
		- Compile time generation for each type
		- Runtime - can't put ONE function in instruction cache of CPU. Because many 'versions' - monomorphized exist! 
- Dispatch - Static vs Dynamic 
	- `pub fn strlen(s: impl Hei)` - static dispatch => get monomorphized to `strlen_str(s: &str)` if `impl Hei for &str `  exists
	- `pub fn strlen_dyn(s: &dyn Hei)` - pointer indirection on stack.
	- `pub fn strlen_dyn(s: Box<dyn Hei>)` - pointer indirection for object on heap
	- 