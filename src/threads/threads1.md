# Exercise 81

- Name: ```threads1```
- Path: ```exercises/threads/threads1.rs```
#### Hint: 

`JoinHandle` is a struct that is returned from a spawned thread:
https://doc.rust-lang.org/std/thread/fn.spawn.html

A challenge with multi-threaded applications is that the main thread can
finish before the spawned threads are completed.
https://doc.rust-lang.org/book/ch16-01-threads.html#waiting-for-all-threads-to-finish-using-join-handles

Use the JoinHandles to wait for each thread to finish and collect their results.
https://doc.rust-lang.org/std/thread/struct.JoinHandle.html



---



```rust,editable
// threads1.rs
//
// This program spawns multiple threads that each run for at least 250ms, and
// each thread returns how much time they took to complete. The program should
// wait until all the spawned threads have finished and should collect their
// return values into a vector.
//
// Execute `rustlings hint threads1` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

use std::thread;
use std::time::{Duration, Instant};

fn main() {
    let mut handles = vec![];
    for i in 0..10 {
        handles.push(thread::spawn(move || {
            let start = Instant::now();
            thread::sleep(Duration::from_millis(250));
            println!("thread {} is complete", i);
            start.elapsed().as_millis()
        }));
    }

    let mut results: Vec<u128> = vec![];
    for handle in handles {
        // TODO: a struct is returned from thread::spawn, can you use it?
    }

    if results.len() != 10 {
        panic!("Oh no! All the spawned threads did not finish!");
    }

    println!();
    for (i, result) in results.into_iter().enumerate() {
        println!("thread {} took {}ms", i, result);
    }
}

```
