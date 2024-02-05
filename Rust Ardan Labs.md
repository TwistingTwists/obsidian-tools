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


## 2. Threads 

* handle and join , move closure (so that the ownership of all the variables is taken by thread)
* Scoped threads
* Atomic and Lazy Initialisation
* Mutex
* RwLock 
* 