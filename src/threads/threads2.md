# Exercise 82

- Name: ```threads2```
- Path: ```exercises/threads/threads2.rs```
#### Hint: 

`Arc` is an Atomic Reference Counted pointer that allows safe, shared access to **immutable** data. But we want to *change* the number of `jobs_completed` so we'll need to also use another type that will only allow one thread to mutate the data at a time. Take a look at this section of the book:

[https://doc.rust-lang.org/book/ch16-03-shared-state.html](https://doc.rust-lang.org/book/ch16-03-shared-state.html#atomic-reference-counting-with-arct) ([Chinese version](https://rustwiki.org/zh-CN/book/ch16-03-shared-state.html#%E5%8E%9F%E5%AD%90%E5%BC%95%E7%94%A8%E8%AE%A1%E6%95%B0-arct))



and keep reading if you'd like more hints :)

Do you now have an `Arc` `Mutex` `JobStatus` at the beginning of main? Like:

`let status = Arc::new(Mutex::new(JobStatus { jobs_completed: 0 }));`

Similar to the code in the example in the book that happens after the text that says "We can use Arc<T> to fix this.". If not, give that a try! If you do and would like more hints, keep reading!!

Make sure neither of your threads are holding onto the lock of the mutex while they are sleeping, since this will prevent the other thread from being allowed to get the lock. Locks are automatically released when
they go out of scope.

If you've learned from the sample solutions, I encourage you to come back to this exercise and try it again in a few days to reinforce what you've learned :)


---



```rust,editable
// threads2.rs
//
// Building on the last exercise, we want all of the threads to complete their
// work but this time the spawned threads need to be in charge of updating a
// shared value: JobStatus.jobs_completed
//
// Execute `rustlings hint threads2` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

use std::sync::Arc;
use std::thread;
use std::time::Duration;

struct JobStatus {
    jobs_completed: u32,
}

fn main() {
    let status = Arc::new(JobStatus { jobs_completed: 0 });
    let mut handles = vec![];
    for _ in 0..10 {
        let status_shared = Arc::clone(&status);
        let handle = thread::spawn(move || {
            thread::sleep(Duration::from_millis(250));
            // TODO: You must take an action before you update a shared value
            status_shared.jobs_completed += 1;
        });
        handles.push(handle);
    }
    for handle in handles {
        handle.join().unwrap();
        // TODO: Print the value of the JobStatus.jobs_completed. Did you notice
        // anything interesting in the output? Do you have to 'join' on all the
        // handles?
        println!("jobs completed {}", ???);
    }
}

```

---

### 本题内容

这个练习深入探讨了如何在多线程环境中安全地更新共享状态。任务是让多个线程协作更新一个共享变量 `jobs_completed`，并确保更新操作的安全性和正确性。

### 相关知识点

1. **`Arc<T>`**：
   - `Arc`（原子引用计数）是一个允许多个线程访问同一数据的智能指针，提供了线程安全的共享数据访问。使用 `Arc` 可以在多个线程之间安全共享数据的所有权。

2. **`Mutex<T>`**：
   - `Mutex`（互斥锁）用于保护共享数据的访问，确保任何时候只有一个线程可以访问数据。当需要修改共享数据时，线程必须首先锁定 `Mutex`，完成数据修改后释放锁。

3. **线程同步**：
   - 管理好线程间的同步是并发编程中的关键挑战之一。使用互斥锁可以同步线程对共享资源的访问，但需要小心处理，以避免死锁或过度锁定资源导致的性能问题。

### 解题方法

#### 步骤描述

1. **初始化共享状态**：
   - 使用 `Arc` 包装一个 `Mutex`，该 `Mutex` 内部包含 `JobStatus` 结构体，以确保跨线程的安全访问和修改。

2. **创建线程**：
   - 生成多个线程，每个线程通过克隆 `Arc` 来共享对 `Mutex` 和 `JobStatus` 的访问。

3. **线程内修改共享状态**：
   - 每个线程在执行任务后，通过锁定 `Mutex` 来安全更新 `jobs_completed`。确保在操作完成后及时释放锁。

4. **收集结果**：
   - 主线程等待所有子线程完成（使用 `JoinHandle`），然后打印最终的 `jobs_completed` 值。

#### 代码示例

```rust
use std::sync::{Arc, Mutex};
use std::thread;
use std::time::Duration;

struct JobStatus {
    jobs_completed: u32,
}

fn main() {
    let status = Arc::new(Mutex::new(JobStatus { jobs_completed: 0 }));
    let mut handles = vec![];

    for _ in 0..10 {
        let status_shared = Arc::clone(&status);
        let handle = thread::spawn(move || {
            thread::sleep(Duration::from_millis(250));
            let mut num = status_shared.lock().unwrap();
            num.jobs_completed += 1;
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }

    // 打印最终结果
    let completed = status.lock().unwrap();
    println!("jobs completed {}", completed.jobs_completed);
}
```

## 扩展知识点与解答：

### 扩展知识点

1. **理解互斥锁的工作原理**：
   - 互斥锁（`Mutex`）确保同一时间内只有一个线程可以访问某个数据。理解互斥锁的工作机制和其在多线程编程中的用途是至关重要的。互斥锁通常与条件变量（`Condvar`）配合使用，用于线程间的协调和状态通知。

2. **避免死锁**：
   - 在使用互斥锁时，程序设计需要小心规避死锁的情况。死锁发生在多个线程相互等待对方释放资源的情况下，导致所有线程都无法继续执行。理解如何正确加锁和释放锁，以及如何设计避免死锁的线程逻辑，是并发编程的重要技能。

3. **使用其他同步原语**：
   - Rust 提供了其他类型的同步原语，如信号量（`Semaphore`）、读写锁（`RwLock`）等。这些工具在特定情况下可以提供比互斥锁更高的性能或更适合的功能特性。例如，`RwLock` 允许多个线程同时读取数据，但写入时需要独占访问。

4. **原子操作和 `Atomic` 类型**：
   - 对于简单的计数或标记，使用原子类型（如 `AtomicUsize`）可能比使用互斥锁更高效。原子操作无需锁定，因此可以减少线程阻塞的时间，适用于高并发场景。

### 扩展解题方法

1. **线程池的使用**：
   - 对于大规模的线程管理，使用线程池可以有效地控制线程的数量和生命周期，提高资源利用率和响应速度。线程池允许你重用一组线程，而不是为每个任务创建和销毁线程。

2. **细粒度锁**：
   - 对于更复杂的并发数据结构，考虑实现细粒度锁的策略，即在数据结构内部使用多个锁。这样可以允许更高程度的并发，尤其是在读多写少的场景中，可以显著提高性能。

3. **锁的范围和持有时间**：
   - 优化锁的范围和持有时间可以减少等待时间和提高并发性能。确保仅在必要时加锁，并尽快释放锁，以减少对其他线程的阻塞。

4. **利用并发工具库**：
   - 探索并使用 Rust 的并发库，如 `rayon`，它提供了数据并行处理的高级抽象，可以简化并行代码的编写，并自动处理线程管理和数据分割。

