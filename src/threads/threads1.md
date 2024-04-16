# Exercise 81

- Name: ```threads1```
- Path: ```exercises/threads/threads1.rs```
#### Hint: 

`JoinHandle` is a struct that is returned from a spawned thread:

[https://doc.rust-lang.org/std/thread/fn.spawn.html](https://doc.rust-lang.org/std/thread/fn.spawn.html) ([Chinese version](https://rustwiki.org/zh-CN/std/thread/fn.spawn.html))

A challenge with multi-threaded applications is that the main thread can finish before the spawned threads are completed.

[https://doc.rust-lang.org/book/ch16-01-threads.html#waiting-for-all-threads-to-finish-using-join-handles](https://doc.rust-lang.org/book/ch16-01-threads.html#waiting-for-all-threads-to-finish-using-join-handles) ([Chinese version](https://rustwiki.org/zh-CN/book/ch16-01-threads.html#%E4%BD%BF%E7%94%A8-join-%E7%AD%89%E5%BE%85%E6%89%80%E6%9C%89%E7%BA%BF%E7%A8%8B%E7%BB%93%E6%9D%9F))

Use the JoinHandles to wait for each thread to finish and collect their results.

[https://doc.rust-lang.org/std/thread/struct.JoinHandle.html](https://doc.rust-lang.org/std/thread/struct.JoinHandle.html) ([Chinese version](https://rustwiki.org/zh-CN/std/thread/struct.JoinHandle.html))



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

---

### 本题内容

这个练习旨在引导你实践多线程编程的基本概念，特别是如何启动线程、等待线程完成，并收集它们的返回值。你将创建多个线程，每个线程模拟执行某项任务，然后主线程需要收集每个子线程执行完毕后的结果。

### 相关知识点

1. **`thread::spawn`**：
   - 用于创建一个新的线程并开始执行给定的闭包函数。
   - 返回一个 `JoinHandle`，它是一个句柄，用于管理对新线程的控制。

2. **`JoinHandle`**：
   - 允许你等待线程的完成（通过调用其 `join()` 方法）并获取线程的返回值。
   - `join()` 会阻塞当前线程直到对应的线程结束。

3. **`Instant` 和 `Duration`**：
   - `Instant` 用于获取一个时间点，通常用于计时。
   - `Duration` 用来表示时间间隔。

### 解题方法

#### 步骤描述

1. **启动多个线程**：
   - 使用 `thread::spawn` 在循环中启动多个线程。
   - 每个线程休眠一定时间（如 250ms），模拟执行任务，并计算它们的执行时间。

2. **收集 `JoinHandle`**：
   - 将每个线程的 `JoinHandle` 收集到一个向量中，以便后续操作。

3. **等待所有线程完成**：
   - 遍历 `JoinHandle` 向量，使用 `join()` 方法等待每个线程完成。
   - 收集每个线程的返回值（执行时间）到结果向量中。

4. **输出结果**：
   - 打印出每个线程的执行时间。

#### 代码示例

```rust
use std::thread;
use std::time::{Duration, Instant};

fn main() {
    let mut handles = vec![];
    for i in 0..10 {
        handles.push(thread::spawn(move || {
            let start = Instant::now();
            thread::sleep(Duration::from_millis(250)); // 模拟工作
            println!("thread {} is complete", i);
            start.elapsed().as_millis() // 返回执行时间
        }));
    }

    let mut results: Vec<u128> = vec![];
    for handle in handles {
        let result = handle.join().unwrap(); // 等待线程完成并获取结果
        results.push(result);
    }

    assert_eq!(results.len(), 10, "Oh no! All the spawned threads did not finish!");

    println!();
    for (i, result) in results.into_iter().enumerate() {
        println!("thread {} took {}ms", i, result);
    }
}
```

示例2，[参见Arc1](../smart_pointers/arc1.html#%E4%BB%A3%E7%A0%81%E7%A4%BA%E4%BE%8B)

## 扩展知识点与解答：

### 扩展知识点

1. **线程的生命周期管理**：
   - 在 Rust 中，每个通过 `thread::spawn` 创建的线程都返回一个 `JoinHandle`。管理这个 `JoinHandle` 是控制线程生命周期的关键。了解如何正确使用 `JoinHandle` 可以帮助你确保所有线程在程序结束前正确完成，避免程序早于其创建的线程结束。

2. **线程间的数据共享**：
   - 虽然本练习主要关注线程的启动和同步，但在实际应用中，线程间的数据共享是一个常见的需求。在 Rust 中，使用像 `Arc`（原子引用计数）这样的智能指针可以安全地在多个线程间共享数据。了解何时以及如何在线程之间共享数据是并发编程的一个重要方面。

3. **错误处理**：
   - 在多线程环境中，适当地处理错误尤为重要。`JoinHandle` 的 `join()` 方法返回一个 `Result` 类型，能够让你捕捉到线程内部发生的错误。合理处理这些错误可以防止程序崩溃并提高程序的健壮性。

4. **线程同步机制**：
   - 除了 `JoinHandle`，Rust 标准库中还提供了多种同步原语，如互斥锁（`Mutex`）、条件变量（`Condvar`）和信号量（`Semaphore`），它们可以用于不同的线程同步场景。了解这些同步机制及其适用场景能够帮助你设计更复杂的多线程程序。

### 扩展解题方法

1. **使用线程池**：
   - 对于大规模的多线程应用，使用线程池可以有效管理线程的创建和销毁，提高资源利用率和性能。考虑使用像 `rayon` 这样的并发数据处理库来简化线程池的实现。

2. **性能监控**：
   - 在多线程程序中监控性能非常重要，可以使用工具如 `perf` 或 `gdb`，或者在 Rust 代码中使用 `Instant` 和 `Duration` 来精确测量代码执行时间。这可以帮助你识别瓶颈并优化程序性能。

3. **基准测试**：
   - 使用 Rust 的基准测试功能来评估不同的线程管理策略和同步机制对性能的影响。这可以通过实验性地修改线程的数量或同步方式来进行。

通过以上扩展知识点和解题方法，你可以更深入地理解和应用 Rust 中的多线程编程技术，从而构建更健壮、高效和可扩展的并发应用程序。
