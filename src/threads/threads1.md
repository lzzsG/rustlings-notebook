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



