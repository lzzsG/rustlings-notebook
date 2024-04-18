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

## 关于 `Mutex` 和 `RwLock`

在 Rust 中，`RwLock` 和 `Mutex` 是两种常见的同步原语，它们都用于控制对共享数据的并发访问。虽然它们的目的相同，即保证数据访问的安全性和一致性，但它们的工作方式和适用场景有所不同。下面我们将深入讲解这两者的特点和差异：

### Mutex（互斥锁）

- **基本功能**：
  `Mutex`（Mutual Exclusion，互斥锁）用于保护共享数据，确保在任何时刻只有一个线程可以访问该数据。当一个线程想要访问由 `Mutex` 保护的数据时，它必须首先获得这个 `Mutex` 的锁。如果锁已被另一个线程持有，该线程将被阻塞，直到锁被释放。

- **使用场景**：
  `Mutex` 适用于读写操作差不多或写操作较多的场景，因为每次只允许一个线程访问数据，无论是读操作还是写操作。

- **性能考量**：
  `Mutex` 可能会导致较高的线程等待时间，特别是在高并发的情况下，因为所有访问都是串行的。

- **API 示例**：
  

```rust
use std::sync::Mutex;

let m = Mutex::new(5);

{
    let mut num = m.lock().unwrap();
    *num = 6;
}

println!("m = {:?}", m.lock().unwrap());
```

### RwLock（读写锁）

- **基本功能**：
  `RwLock`（Read-Write Lock，读写锁）允许更灵活的数据访问方式。它区分了读操作和写操作：允许多个线程同时读取数据，但写操作要求独占访问。如果一个线程持有写锁，其他线程的读写操作都会被阻塞；如果有多个线程持有读锁，则任何试图获得写锁的线程都会被阻塞。

- **使用场景**：
  `RwLock` 特别适合读多写少的场景。在这种情况下，读写锁可以提高并发性，因为它允许多个读操作同时进行而不阻塞彼此。

- **性能考量**：
  尽管 `RwLock` 在读多写少的情况下表现更好，但它的管理开销通常比 `Mutex` 更高，因为需要同时管理读者和写者的状态。

- **API 示例**：
  

```rust
use std::sync::RwLock;

let lock = RwLock::new(5);

{
    let r1 = lock.read().unwrap();
    let r2 = lock.read().unwrap();
    println!("Readers see: {}", *r1);
    // r1 和 r2 是同时持有的读锁
}

{
    let mut w = lock.write().unwrap();
    *w += 1;
    println!("Writer writes: {}", *w);
}
```

`Mutex` 和 `RwLock` 都是用于控制并发访问共享数据的重要工具。选择使用哪一个取决于您的具体需求：如果写操作频繁或读写操作差不多频繁，使用 `Mutex`；如果系统明显读操作多于写操作，考虑使用 `RwLock` 来提高读取效率。在决定使用哪一个之前，了解您的数据访问模式和性能需求是非常重要的。

---

## 并发编程以及相关的数据管理

并发编程是现代软件开发中的一个核心主题，特别是在多核处理器普及的今天。Rust 通过提供一系列的并发原语，使得在保持高性能的同时，还能确保内存安全和线程安全。这些原语包括线程、通道、共享状态、原子操作等。下面，我们将深入讲解 Rust 中的并发编程以及相关的数据管理。

### 线程

Rust 使用线程来实现并发。在 Rust 中，线程可以通过 `std::thread` 模块来创建和管理。Rust 提供的线程是 1:1 模型，即一个 Rust 线程对应操作系统的一个线程。

**创建和使用线程：**
```rust
use std::thread;
use std::time::Duration;

fn main() {
    let handle = thread::spawn(|| {
        for i in 1..10 {
            println!("hi number {} from the spawned thread!", i);
            thread::sleep(Duration::from_millis(1));
        }
    });

    for i in 1..5 {
        println!("hi number {} from the main thread!", i);
        thread::sleep(Duration::from_millis(1));
    }

    handle.join().unwrap();
}
```

在这个例子中，`spawn` 函数用于创建一个新线程，`join` 方法确保主线程会等待新线程完成。

### 通道

通道（Channels）是 Rust 中进行线程间通信的一种方式。Rust 的通道有两个部分：发送端和接收端。发送端用于发送数据，接收端用于接收数据。通道是多生产者，单消费者（MPSC）模型。

**创建和使用通道：**
```rust
use std::sync::mpsc;
use std::thread;
use std::time::Duration;

fn main() {
    let (tx, rx) = mpsc::channel();

    thread::spawn(move || {
        let val = String::from("hello");
        tx.send(val).unwrap();
        // 注意这里不能再使用 val，因为值已经发送走了
    });

    let received = rx.recv().unwrap();
    println!("Got: {}", received);
}
```

### 共享状态

共享内存是另一种线程间通信的方式。如前所述，Rust 提供了多种同步原语来安全地管理共享内存，比如 `Mutex` 和 `RwLock`。

**使用 Mutex 共享状态：**
```rust
use std::sync::{Arc, Mutex};
use std::thread;

fn main() {
    let counter = Arc::new(Mutex::new(0));
    let mut handles = vec![];

    for _ in 0..10 {
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

    println!("Result: {}", *counter.lock().unwrap());
}
```

### 原子操作

原子类型是另一种管理共享数据的方法，但它们提供无锁的线程安全。在 Rust 中，原子操作是通过 `std::sync::atomic` 模块实现的，它支持多种原子类型如 `AtomicBool`, `AtomicI32` 等。

**使用原子操作：**
```rust
use std::sync::atomic::{AtomicUsize, Ordering};
use std::sync::Arc;
use std::thread;

fn main() {
    let val = Arc::new(AtomicUsize::new(0));
    let mut handles = vec![];

    for _ in 0..10 {
        let val = Arc::clone(&val);
        let handle = thread::spawn(move || {
            for _ in 0..1000 {
                val.fetch_add(1, Ordering::SeqCst);
            }
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }

    println!("Result: {}", val.load(Ordering::SeqCst));
}
```

这只是并发编程的基础。在实际应用中，还需要考虑锁的粒度、避免死锁、避免竞态条件等高级话题。Rust 的所有权和借用规则在编译时就能帮助我们避免许多并发相关的错误，使得 Rust 在并发编程领域表现出色。

在 Rust 的并发编程中，除了基本的线程和同步原语，还有一些更实用的技术和用法，包括 `lazy_static!` 宏、条件变量、屏障以及跨线程的错误处理等。以下我们将深入探讨这些技术和用法。

### lazy_static! 宏

`lazy_static!` 宏允许我们定义在首次访问时才初始化的静态变量。这对于复杂的静态初始化或需要运行时数据来初始化的情况非常有用。此外，`lazy_static!` 也常用于创建全局可访问的资源，如配置对象或应用级的状态。

**实例：使用 `lazy_static!` 创建配置单例**
```rust
#[macro_use]
extern crate lazy_static;

use std::collections::HashMap;

lazy_static! {
    static ref CONFIG: HashMap<String, String> = {
        let mut m = HashMap::new();
        m.insert("hostname".to_string(), "localhost".to_string());
        m.insert("port".to_string(), "8080".to_string());
        m
    };
}

fn main() {
    println!("Hostname: {}", CONFIG.get("hostname").unwrap());
    println!("Port: {}", CONFIG.get("port").unwrap());
}
```

这个例子中，`CONFIG` 在第一次被访问时初始化，并在程序的剩余生命周期中保持不变。这种方式特别适用于存储应用配置和环境变量。

### 条件变量（Condition Variables）

条件变量是与互斥锁（Mutex）配合使用的，允许线程在某些条件下挂起，并在条件为真时被唤醒。条件变量主要用于线程间的协调和同步。

**实例：使用条件变量同步线程**
```rust
use std::sync::{Arc, Mutex, Condvar};
use std::thread;

fn main() {
    let pair = Arc::new((Mutex::new(false), Condvar::new()));
    let pair_clone = Arc::clone(&pair);

    thread::spawn(move || {
        let (lock, cvar) = &*pair_clone;
        let mut started = lock.lock().unwrap();
        *started = true;
        cvar.notify_one(); // 唤醒一个等待线程
    });

    let (lock, cvar) = &*pair;
    let mut started = lock.lock().unwrap();
    while !*started {
        started = cvar.wait(started).unwrap();
    }

    println!("Thread is started!");
}
```

这里使用条件变量来等待子线程设置 `started` 标志为 `true`，主线程在条件满足前会一直等待。

### 屏障（Barriers）

屏障是一种同步机制，用来确保多个线程在继续执行前都达到某个点。这在并行计算中非常有用，可以用来同步分阶段执行的任务。

**实例：使用屏障同步多个线程**
```rust
use std::sync::{Arc, Barrier};
use std::thread;

fn main() {
    let barrier = Arc::new(Barrier::new(3));
    let mut handles = vec![];

    for _ in 0..3 {
        let c = barrier.clone();
        let handle = thread::spawn(move|| {
            println!("Before wait");
            c.wait();  // 等待所有线程都调用 wait
            println!("After wait");
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }
}
```

### 跨线程的错误处理

在多线程环境中，处理错误需要特别的策略，因为错误可能在任何一个线程中发生，并需要适当地传递到可以处理它们的地方。

**实例：线程间错误传递**
```rust
use std::thread;
use std::sync::mpsc;
use std::result::Result;

fn main() {
    let (tx, rx) = mpsc::channel();

    let handle = thread::spawn(move || {
        let result: Result<(), &'static str> = Err("oops!");
        tx

.send(result).unwrap();
    });

    match rx.recv().unwrap() {
        Ok(_) => println!("Operation successful"),
        Err(e) => println!("Error: {}", e),
    }

    handle.join().unwrap();
}
```

这些技术和用法展示了 Rust 的并发编程能力不仅强大而且灵活。理解和正确使用这些工具将有助于构建高效、可靠和安全的并发应用程序。
