# Exercise 79

- Name: ```arc1```
- Path: ```exercises/smart_pointers/arc1.rs```
#### Hint: 

Make `shared_numbers` be an `Arc` from the numbers vector. Then, in order to avoid creating a copy of `numbers`, you'll need to create `child_numbers` inside the loop but still in the main thread.

`child_numbers` should be a clone of the Arc of the numbers instead of a thread-local copy of the numbers.

This is a simple exercise if you understand the underlying concepts, but if this is too much of a struggle, consider reading through all of Chapter 16 in the book:

[https://doc.rust-lang.org/stable/book/ch16-00-concurrency.html](https://doc.rust-lang.org/stable/book/ch16-00-concurrency.html) ([Chinese version](https://rustwiki.org/zh-CN/book/ch16-00-concurrency.html))

---

```rust,editable
// arc1.rs
//
// In this exercise, we are given a Vec of u32 called "numbers" with values
// ranging from 0 to 99 -- [ 0, 1, 2, ..., 98, 99 ] We would like to use this
// set of numbers within 8 different threads simultaneously. Each thread is
// going to get the sum of every eighth value, with an offset.
//
// The first thread (offset 0), will sum 0, 8, 16, ...
// The second thread (offset 1), will sum 1, 9, 17, ...
// The third thread (offset 2), will sum 2, 10, 18, ...
// ...
// The eighth thread (offset 7), will sum 7, 15, 23, ...
//
// Because we are using threads, our values need to be thread-safe.  Therefore,
// we are using Arc.  We need to make a change in each of the two TODOs.
//
// Make this code compile by filling in a value for `shared_numbers` where the
// first TODO comment is, and create an initial binding for `child_numbers`
// where the second TODO comment is. Try not to create any copies of the
// `numbers` Vec!
//
// Execute `rustlings hint arc1` or use the `hint` watch subcommand for a hint.

// I AM NOT DONE

#![forbid(unused_imports)] // Do not change this, (or the next) line.
use std::sync::Arc;
use std::thread;

fn main() {
    let numbers: Vec<_> = (0..100u32).collect();
    let shared_numbers = // TODO
    let mut joinhandles = Vec::new();

    for offset in 0..8 {
        let child_numbers = // TODO
        joinhandles.push(thread::spawn(move || {
            let sum: u32 = child_numbers.iter().filter(|&&n| n % 8 == offset).sum();
            println!("Sum of offset {} is {}", offset, sum);
        }));
    }
    for handle in joinhandles.into_iter() {
        handle.join().unwrap();
    }
}
```

---

### 本题内容

这个练习旨在展示如何使用 `Arc<T>`（原子引用计数）智能指针在多线程环境中安全地共享数据。任务是将一个数字列表分发给多个线程，每个线程计算列表中特定偏移的数字之和。通过这个练习，你将学习如何在多线程程序中共享数据，而不会引发数据竞争或其他并发错误。

### 相关知识点

1. **`Arc<T>`**：
   - `Arc<T>` 类似于 `Rc<T>`，但它是线程安全的。它允许在多个线程间共享数据的所有权，同时确保引用计数的操作是原子的。

2. **多线程数据共享**：
   - 在多线程应用中共享数据时，必须确保数据访问的线程安全性。`Arc<T>` 提供了一种方法来共享数据，而无需担心内部可变性导致的问题。

3. **线程的创建与管理**：
   - 使用 `thread::spawn` 创建新线程，并通过线程句柄管理线程的生命周期。线程句柄允许你在适当的时候等待线程的完成。

### 解题方法

#### 步骤描述：

1. **初始化 `Arc<T>`**：
   - 创建一个 `Arc<T>` 实例来包装 `numbers` 向量，使其能够在多个线程中安全共享。

2. **克隆 `Arc<T>` 传递到线程**：
   - 在创建每个线程前，克隆 `Arc<T>` 的实例。这样每个线程都拥有对数据的共享访问权，但不会增加原始数据的复制。

3. **线程中使用共享数据**：
   - 在每个线程中，使用克隆得到的 `Arc<T>` 实例来访问共享的向量，并根据偏移量计算和。

4. **等待所有线程完成**：
   - 使用 `join` 方法等待所有线程执行完毕。这确保主线程会在所有工作线程完成工作后才继续执行。

#### 代码示例：

```rust
use std::sync::Arc;
use std::thread;

fn main() {
    let numbers: Vec<_> = (0..100u32).collect();
    let shared_numbers = Arc::new(numbers);  // 使用 Arc 包装 numbers
    let mut joinhandles = Vec::new();

    for offset in 0..8 {
        let child_numbers = Arc::clone(&shared_numbers);  // 克隆 Arc 传递到线程中
        joinhandles.push(thread::spawn(move || {
            let sum: u32 = child_numbers
                .iter()
                .enumerate()
                .filter(|&(i, _)| i % 8 == offset)
                .map(|(_, &n)| n)
                .sum();
            println!("Sum of offset {} is {}", offset, sum);
        }));
    }

    for handle in joinhandles.into_iter() {
        handle.join().unwrap();  // 等待所有线程完成
    }
}
```

这个示例正确地使用了 `Arc<T>` 来实现多线程下的数据共享，每个线程都能安全地访问共享数据而不会引起竞争条件。此外，使用 `Arc::clone` 只是增加了引用计数，并没有复制底层数据，这使得程序既安全又高效。

### 代码解释：

这段代码展示了如何使用 `Arc`（Atomic Reference Counted）来在多个线程间共享数据，并使用 Rust 标准库中的线程进行并发计算。下面逐行解释代码的功能和逻辑：

#### 导入必要的模块

```rust
use std::sync::Arc;
use std::thread;
```

这两行导入了 Rust 标准库中的 `Arc` 和 `thread` 模块。`Arc` 用于创建原子引用计数的智能指针，允许在多个线程之间安全共享数据。`thread` 模块用于操作线程。

#### 主函数

```rust
fn main() {
    let numbers: Vec<_> = (0..100u32).collect();
    let shared_numbers = Arc::new(numbers);  // 使用 Arc 包装 numbers
    let mut joinhandles = Vec::new();
```

- `let numbers: Vec<_> = (0..100u32).collect();` 这行代码创建了一个包含 0 到 99 的整数的向量。
- `let shared_numbers = Arc::new(numbers);` 这行代码将 `numbers` 向量包装在一个 `Arc` 中，使得这个向量可以被多个线程安全地共享。
- `let mut joinhandles = Vec::new();` 初始化一个向量来存储线程句柄，这些句柄将用于稍后等待线程完成。

#### 创建线程

```rust
for offset in 0..8 {
    let child_numbers = Arc::clone(&shared_numbers);  // 克隆 Arc 传递到线程中
    joinhandles.push(thread::spawn(move || {
        let sum: u32 = child_numbers
            .iter()
            .enumerate()
            .filter(|&(i, _)| i % 8 == offset)
            .map(|(_, &n)| n)
            .sum();
        println!("Sum of offset {} is {}", offset, sum);
    }));
}
```

这段代码通过循环创建了 8 个线程。每个线程都计算特定偏移量的数字之和：
- `let child_numbers = Arc::clone(&shared_numbers);` 这行代码克隆了 `Arc` 指针，每个线程获得 `shared_numbers` 的一个引用，不会增加原始数据的复制，只增加引用计数。
- `thread::spawn(move || {...})` 用于生成一个新线程。`move` 关键字用于将闭包中使用的变量（在这里是 `child_numbers` 和 `offset`）的所有权转移到线程内部。
- 在闭包内部，使用链式调用来计算符合条件的元素之和。具体来说：
  - `.enumerate()` 为向量的迭代器添加索引。
  - `.filter(|&(i, _)| i % 8 == offset)` 筛选出索引符合当前偏移量的元素。
  - `.map(|(_, &n)| n)` 从元组中提取数字。
  - `.sum()` 计算筛选后元素的总和。
  - `println!("Sum of offset {} is {}", offset, sum);` 打印计算结果。

#### 等待线程完成

```rust
for handle in joinhandles.into_iter() {
    handle.join().unwrap();  // 等待所有线程完成
}
```

- 这段代码遍历所有的线程句柄，使用 `.join()` 方法等待每个线程完成执行。`unwrap()` 用于处理可能的错误，例如线程因某种原因而崩溃。

## 扩展知识点与解答：

### 扩展知识点

1. **原子操作的性能考量**：
   - 使用 `Arc<T>` 时，引用计数的增减是通过原子操作实现的。虽然原子操作比非原子操作消耗更多的资源，但它们是确保线程安全的必要手段。了解原子操作的开销及其对性能的影响可以帮助你更好地设计高效的多线程程序。

2. **内存顺序与 `Arc<T>`**：
   - 在使用原子类型时，Rust 允许指定不同的内存顺序，这影响程序的性能和正确性。虽然 `Arc<T>` 默认使用顺序一致性模型（最严格的内存顺序），但在某些用例中，了解其他内存顺序如 `Relaxed`、`Acquire` 和 `Release` 可能有助于优化性能。

3. **与其他同步和并发工具的结合使用**：
   - `Arc<T>` 通常与互斥锁（如 `Mutex<T>`）和信号量（如 `Semaphore`）等同步原语结合使用，以控制对共享数据的访问。掌握这些组合的使用是构建复杂并发应用的关键。

### 扩展解题方法

1. **使用 `Arc<Mutex<T>>`**：
   - 在需要修改通过 `Arc<T>` 共享的数据时，可以将 `Mutex<T>` 和 `Arc<T>` 结合使用。这样不仅可以安全地在多个线程间共享数据，还可以修改它。

2. **优化内存顺序**：
   - 在不牺牲安全性的前提下，调整 `Arc<T>` 的内存顺序设置可能提高性能，尤其是在读多写少的情况下。

3. **监控和调试多线程程序**：
   - 使用 Rust 的调试工具和库（如 `rayon`）监控 `Arc<T>` 的使用情况，检测潜在的性能瓶颈和内存泄漏。

4. **实践案例：构建高并发服务器**：
   - 使用 `Arc<T>` 来管理服务器中的共享资源，例如连接池、缓存或配置数据。这可以确保高效的资源利用和良好的扩展性。

### 示例：使用 `Arc<Mutex<T>>`

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

这个示例展示了如何在多线程中安全地共享和修改一个整数，使用 `Arc<Mutex<T>>` 组合确保了线程安全的同时提供了修改能力。这种模式在构建需要线程安全写操作的系统中非常有用。

## 关于多线程

在上面的讨论中，我们接触到了多线程编程的几个关键概念，特别是在 Rust 语言的上下文中。这些概念为我们在后续深入学习多线程应用提供了基础。以下是涉及的主要内容简介：

### 1. **多线程数据共享**
在多线程程序中，数据共享是一个常见需求。为了安全地在多个线程间共享数据，Rust 提供了几种智能指针类型，如 `Arc<T>` 和 `Mutex<T>`。这些工具帮助我们管理内存和同步访问，防止数据竞争和其他并发错误。

### 2. **`Arc<T>`**
`Arc<T>`，即原子引用计数（Atomic Reference Counted），是一种允许多个线程拥有某些数据的智能指针。它通过原子操作来管理引用计数，保证了线程安全。当你需要在多个线程之间共享数据的只读访问权限时，`Arc<T>` 非常有用。

### 3. **`Mutex<T>`**
`Mutex<T>` 是一种基本的同步原语，用于在多线程环境中保护共享数据。`Mutex`（互斥锁）保证了在任何时刻，只有一个线程可以访问某些数据。当需要修改共享数据时，通常会将 `Mutex<T>` 与 `Arc<T>` 结合使用，以安全地跨线程修改数据。

### 4. **线程的创建和管理**
在 Rust 中，可以使用 `std::thread` 模块来创建和管理线程。线程一旦创建，你可以通过线程句柄（handle）来控制它的行为，例如等待一个线程完成执行。线程的创建和管理是并发编程中的基本技能。

### 5. **线程安全和内存顺序**
Rust 强制实施线程安全，确保不会发生数据竞争。此外，Rust 在底层还支持精细控制内存顺序，这对于需要高性能同步操作的应用程序非常重要。

在后续的 threads 讲解中，我们将更深入地探索这些主题，包括更复杂的同步机制、线程间的通信方式（如通道）、以及实战中的多线程应用案例。这些内容将帮助你更全面地理解并有效地应用 Rust 的多线程编程能力。
