# threads

---

这一系列的练习涵盖了 Rust 中的多线程编程，从基础的线程创建和同步到更高级的共享状态管理和消息传递。以下是针对每个练习的简要介绍：

#### 练习 81: 线程的基础和同步

在这个练习中，你将学习如何创建多个线程并在它们完成执行后收集结果。这是学习多线程编程的基础，特别是理解如何同步线程以确保所有线程都按预期完成。通过实现，你将更好地理解如何使用 `JoinHandle` 来等待线程完成。

#### 练习 82: 共享状态的线程安全访问

这个练习深入探讨了如何安全地在多个线程间共享和更新状态。使用 `Arc` 和 `Mutex`，你将实现一个安全的更新共享状态的模式，这对于构建需要线程协作的复杂系统尤其重要。这将帮助你理解互斥锁的使用和线程间的数据保护。

#### 练习 83: 多生产者单消费者（mpsc）模式

本练习引入了使用 `mpsc` 通道进行线程间通信的概念。你将学习如何创建一个通道，让多个线程（生产者）向它发送数据，并在主线程（消费者）中接收这些数据。这是学习 Rust 异步编程和并发通信模式的重要一步。

这些练习将帮助你构建坚实的多线程编程基础，为更复杂的并发和并行编程任务做好准备。

---

```rust
// 你不必现在理解以下代码，不过你可以尝试运行它。

use std::sync::{Arc, Mutex, mpsc};
use std::thread;

fn main() {
    let handles = (1..=3).map(|i| {
        thread::spawn(move || {
            println!("Elf {} is solving puzzle {}", i, i);
            thread::sleep(std::time::Duration::from_millis(100 * i as u64));
            i * 2 
        })
    }).collect::<Vec<_>>();

    for handle in handles {
        println!("Puzzle solved with energy: {}", handle.join().unwrap());
    }

    let counter = Arc::new(Mutex::new(0));
    let mut handles = vec![];

    for _ in 0..5 {
        let counter = Arc::clone(&counter);
        let handle = thread::spawn(move || {
            let mut num = counter.lock().unwrap();
            *num += 1;
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }

    println!("Total count of solved puzzles: {}", *counter.lock().unwrap());

    let (tx, rx) = mpsc::channel();
    let mut handles = vec![];

    for i in 1..=5 {
        let tx = tx.clone();
        let handle = thread::spawn(move || {
            tx.send(i * 2).unwrap();
            println!("The elves are producing energy: {}", i * 2);
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }

    drop(tx); 

    while let Ok(value) = rx.recv() {
        println!("Energy received: {}", value);
    }

    println!("\nYour journey continues into🌲🌲the dark forest.🌲🌲\nStarlight filters through the tree canopy, promising guidance and protection on\nyour path ahead.");
}
```
