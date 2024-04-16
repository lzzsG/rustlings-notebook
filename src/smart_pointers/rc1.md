# Exercise 78

- Name: ```rc1```
- Path: ```exercises/smart_pointers/rc1.rs```
#### Hint: 

This is a straightforward exercise to use the Rc<T> type. Each Planet has ownership of the Sun, and uses Rc::clone() to increment the reference count of the Sun.

After using drop() to move the Planets out of scope individually, the reference count goes down. In the end the sun only has one reference again, to itself. See more at:

[https://doc.rust-lang.org/book/ch15-04-rc.html](https://doc.rust-lang.org/book/ch15-04-rc.html) ([Chinese version](https://rustwiki.org/zh-CN/book/ch15-04-rc.html))

* Unfortunately Pluto is no longer considered a planet :(



---



```rust,editable
// rc1.rs
//
// In this exercise, we want to express the concept of multiple owners via the
// Rc<T> type. This is a model of our solar system - there is a Sun type and
// multiple Planets. The Planets take ownership of the sun, indicating that they
// revolve around the sun.
//
// Make this code compile by using the proper Rc primitives to express that the
// sun has multiple owners.
//
// Execute `rustlings hint rc1` or use the `hint` watch subcommand for a hint.

// I AM NOT DONE

use std::rc::Rc;

#[derive(Debug)]
struct Sun {}

#[derive(Debug)]
enum Planet {
    Mercury(Rc<Sun>),
    Venus(Rc<Sun>),
    Earth(Rc<Sun>),
    Mars(Rc<Sun>),
    Jupiter(Rc<Sun>),
    Saturn(Rc<Sun>),
    Uranus(Rc<Sun>),
    Neptune(Rc<Sun>),
}

impl Planet {
    fn details(&self) {
        println!("Hi from {:?}!", self)
    }
}

fn main() {
    let sun = Rc::new(Sun {});
    println!("reference count = {}", Rc::strong_count(&sun)); // 1 reference

    let mercury = Planet::Mercury(Rc::clone(&sun));
    println!("reference count = {}", Rc::strong_count(&sun)); // 2 references
    mercury.details();

    let venus = Planet::Venus(Rc::clone(&sun));
    println!("reference count = {}", Rc::strong_count(&sun)); // 3 references
    venus.details();

    let earth = Planet::Earth(Rc::clone(&sun));
    println!("reference count = {}", Rc::strong_count(&sun)); // 4 references
    earth.details();

    let mars = Planet::Mars(Rc::clone(&sun));
    println!("reference count = {}", Rc::strong_count(&sun)); // 5 references
    mars.details();

    let jupiter = Planet::Jupiter(Rc::clone(&sun));
    println!("reference count = {}", Rc::strong_count(&sun)); // 6 references
    jupiter.details();

    // TODO
    let saturn = Planet::Saturn(Rc::new(Sun {}));
    println!("reference count = {}", Rc::strong_count(&sun)); // 7 references
    saturn.details();

    // TODO
    let uranus = Planet::Uranus(Rc::new(Sun {}));
    println!("reference count = {}", Rc::strong_count(&sun)); // 8 references
    uranus.details();

    // TODO
    let neptune = Planet::Neptune(Rc::new(Sun {}));
    println!("reference count = {}", Rc::strong_count(&sun)); // 9 references
    neptune.details();

    assert_eq!(Rc::strong_count(&sun), 9);

    drop(neptune);
    println!("reference count = {}", Rc::strong_count(&sun)); // 8 references

    drop(uranus);
    println!("reference count = {}", Rc::strong_count(&sun)); // 7 references

    drop(saturn);
    println!("reference count = {}", Rc::strong_count(&sun)); // 6 references

    drop(jupiter);
    println!("reference count = {}", Rc::strong_count(&sun)); // 5 references

    drop(mars);
    println!("reference count = {}", Rc::strong_count(&sun)); // 4 references

    // TODO
    println!("reference count = {}", Rc::strong_count(&sun)); // 3 references

    // TODO
    println!("reference count = {}", Rc::strong_count(&sun)); // 2 references

    // TODO
    println!("reference count = {}", Rc::strong_count(&sun)); // 1 reference

    assert_eq!(Rc::strong_count(&sun), 1);
}
```

---

### 本题内容

本练习的目的是通过实现一个模型来描述我们的太阳系，其中多个行星围绕同一个太阳公转。这里的关键概念是理解如何使用 `Rc<T>`（引用计数智能指针）来表示多个所有权的情况。在这个模型中，每个行星都“拥有”对太阳的引用，表明它们与太阳的关系。通过这个练习，你将学习如何正确地使用 `Rc<T>` 来管理共享数据。

### 相关知识点

1. **`Rc<T>`**:
   - `Rc<T>`，即引用计数指针，用于在程序的多个部分之间共享数据，而不必复制数据。每次克隆 `Rc<T>` 时，它内部的计数会增加，每次一个 `Rc<T>` 被丢弃时，计数会减少。只有当计数归零时，内部数据才会被真正删除。

2. **使用 `Rc::clone` 克隆智能指针**：
   - 使用 `Rc::clone(&rc)` 并不会复制内部的数据，而只是增加了引用计数。这是一种效率很高的操作，允许多个部分持有数据的引用而无需频繁的内存复制。

3. **内存管理**：
   - `Rc<T>` 只能用于单线程场景。对于多线程情况，应使用其线程安全的变体 `Arc<T>`。

### 解题方法

#### 步骤描述：

1. **修改 `Planet` 枚举的定义**：
   - 在每个行星的枚举变体中使用 `Rc<Sun>` 而不是 `Sun`，确保可以共享对太阳的引用。

2. **在 `main` 函数中创建和使用 `Rc<Sun>`**：
   - 创建一个 `Rc<Sun>` 对象表示太阳。
   - 使用 `Rc::clone(&sun)` 为每个行星创建一个指向太阳的引用。

3. **处理引用计数和对象销毁**：
   - 使用 `drop()` 函数显式地丢弃行星，观察和管理引用计数的变化。
   - 确保在程序的最后，太阳的引用计数恢复到 1。

#### 代码示例：

```rust
use std::rc::Rc;

#[derive(Debug)]
struct Sun {}

#[derive(Debug)]
enum Planet {
    Mercury(Rc<Sun>),
    Venus(Rc<Sun>),
    Earth(Rc<Sun>),
    Mars(Rc<Sun>),
    Jupiter(Rc<Sun>),
    Saturn(Rc<Sun>),
    Uranus(Rc<Sun>),
    Neptune(Rc<Sun>),
}

impl Planet {
    fn details(&self) {
        println!("Hi from {:?}!", self)
    }
}

fn main() {
    let sun = Rc::new(Sun {});
    let mercury = Planet::Mercury(Rc::clone(&sun));
    let venus = Planet::Venus(Rc::clone(&sun));
    let earth = Planet::Earth(Rc::clone(&sun));
    let mars = Planet::Mars(Rc::clone(&sun));
    let jupiter = Planet::Jupiter(Rc::clone(&sun));
    let saturn = Planet::Saturn(Rc::clone(&sun));
    let uranus = Planet::Uranus(Rc::clone(&sun));
    let neptune = Planet::Neptune(Rc::clone(&sun));

    // Dropping planets
    drop(mercury);
    drop(venus);
    drop(earth);
    drop(mars);
    drop(jupiter);
    drop(saturn);
    drop(uranus);
    drop(neptune);

    assert_eq!(Rc::strong_count(&sun), 1); // Only one reference left
    println!("Final reference count = {}", Rc::strong_count(&sun));
}
```

通过完成这个练习，你将深入理解 `Rc<T>` 的用法及其在管理共享资源时的重要性，这对构建复杂的数据结构和资源共享模型非常有帮助。

## 扩展知识点与解答：

### 扩展知识点

1. **循环引用和内存泄漏**：
   - 使用 `Rc<T>` 时需要注意循环引用的问题。如果两个对象互相持有对方的 `Rc` 引用，它们的引用计数永远不会达到零，从而导致内存泄漏。理解如何识别和避免这种情况是非常重要的。

2. **`Weak<T>` 指针**：
   - 与 `Rc<T>` 相关的 `Weak<T>` 类型用于创建非拥有引用。`Weak<T>` 引用不会增加引用计数，可用于解决循环引用的问题。了解何时使用 `Weak<T>` 可以帮助你设计更健壮的应用。

3. **内存管理策略**：
   - 在设计使用共享数据的系统时，选择合适的内存管理策略至关重要。理解不同智能指针（如 `Rc`, `Arc`, `Box`）的特点和使用场景，可以帮助你根据应用的需求做出最佳决策。

### 扩展解题方法

1. **解决循环引用**：
   - 设计一个示例来展示 `Rc` 和 `Weak` 的结合使用，如在使用观察者模式时，观察者使用 `Weak<T>` 来避免和主体之间的循环引用。

2. **多线程环境中的引用计数**：
   - 使用 `Arc<T>` 替换 `Rc<T>`，演示如何在多线程环境中安全地共享对象。`Arc<T>` 类似于 `Rc<T>`，但它是线程安全的。

3. **资源管理模拟**：
   - 创建一个模拟系统，其中多个实体（如数据库连接）需要被多个消费者共享。展示如何通过 `Rc<T>` 管理这些共享资源，并在资源不再需要时正确释放。

### `Rc<T>` 的背后机制

1. **引用计数管理**：
   - `Rc<T>` 内部维护了一个引用计数器，用来跟踪有多少个指针指向同一块数据。每次调用 `Rc::clone` 时，内部的引用计数会增加；当 `Rc<T>` 的实例被销毁时，计数会减少。当计数降到零时，`Rc<T>` 会负责清理数据。

2. **不变性保证**：
   - `Rc<T>` 只提供对数据的共享不可变引用。这意味着，尽管可以有多个读取者，但你不能通过 `Rc<T>` 修改其内部的数据。如果需要修改，可以结合使用 `RefCell<T>` 来达到内部可变性。

3. **线程安全**：
   - `Rc<T>` 本身不是线程安全的，因为其操作并不是原子的。在多线程环境下，应该使用 `Arc<T>`，它是 `Rc<T>` 的线程安全等价物，使用原子操作来管理引用计数。

### 扩展用法

1. **与 `RefCell<T>` 结合：**
   - 在需要修改由多个所有者共享的数据时，`Rc<T>` 可以与 `RefCell<T>` 结合使用。`RefCell<T>` 提供了运行时借用检查的能力，允许在运行时通过借用规则来借用可变或不可变引用。


```rust
use std::rc::Rc;
use std::cell::RefCell;

let value = Rc::new(RefCell::new(5));

let a = Rc::clone(&value);
let b = Rc::clone(&value);

*a.borrow_mut() += 1;
println!("a: {}, b: {}", *a.borrow(), *b.borrow());
```

2. **构建复杂的数据结构**：

- `Rc<T>` 非常适合构建如图、树等复杂的数据结构，其中元素可能被多个其他元素共享。

```rust
use std::rc::Rc;

struct Node<T> {
    value: T,
    next: Option<Rc<Node<T>>>
}

let first = Rc::new(Node { value: 10, next: None });
let second = Rc::new(Node { value: 20, next: Some(Rc::clone(&first)) });
```

3. **内存泄漏的防范**：

- 以下示例展示了如何使用 `Weak<T>` 来避免循环引用问题：

```rust
use std::rc::{Rc, Weak};
use std::cell::RefCell;

#[derive(Debug)]
struct Node {
    value: i32,
    parent: RefCell<Weak<Node>>,
    children: RefCell<Vec<Rc<Node>>>,
}

fn main() {
    let leaf = Rc::new(Node {
        value: 3,
        parent: RefCell::new(Weak::new()),
        children: RefCell::new(vec![]),
    });

    let branch = Rc::new(Node {
        value: 5,
        parent: RefCell::new(Weak::new()),
        children: RefCell::new(vec![Rc::clone(&leaf)]),
    });

    *leaf.parent.borrow_mut() = Rc::downgrade(&branch);

    println!("leaf parent = {:?}", leaf.parent.borrow().upgrade());
    println!("branch children = {:?}", branch.children.borrow());
}
```

在这个例子中，`leaf` 节点持有对 `branch` 节点的非拥有（`Weak`）引用，而 `branch` 节点拥有对 `leaf` 的拥有（`Rc`）引用。这种设计避免了循环引用的问题，并且允许 `branch` 被正确地销毁，即使 `leaf` 依然存在。

通过这些扩展知识点和使用方法，可以看出 `Rc<T>` 是 Rust 中管理共享数据的一个非常重要的工具，特别是在单线程应用中。正确的使用和结合其他类型，如 `RefCell<T>` 和 `Weak<T>`，可以有效管理内存并构建功能强大的应用。

