# Exercise 77

- Name: ```box1```
- Path: ```exercises/smart_pointers/box1.rs```
#### Hint: 

Step 1

The compiler's message should help: since we cannot store the value of the actual type
when working with recursive types, we need to store a reference (pointer) to its value.
We should, therefore, place our `List` inside a `Box`. More details in the book here:

[https://doc.rust-lang.org/book/ch15-01-box.html](https://doc.rust-lang.org/book/ch15-01-box.html#enabling-recursive-types-with-boxes) ([Chinese version](https://rustwiki.org/zh-CN/book/ch15-01-box.html))

Step 2

Creating an empty list should be fairly straightforward (hint: peek at the assertions).
For a non-empty list keep in mind that we want to use our Cons "list builder".
Although the current list is one of integers (i32), feel free to change the definition
and try other types!



---



```rust,editable
// box1.rs
//
// At compile time, Rust needs to know how much space a type takes up. This
// becomes problematic for recursive types, where a value can have as part of
// itself another value of the same type. To get around the issue, we can use a
// `Box` - a smart pointer used to store data on the heap, which also allows us
// to wrap a recursive type.
//
// The recursive type we're implementing in this exercise is the `cons list` - a
// data structure frequently found in functional programming languages. Each
// item in a cons list contains two elements: the value of the current item and
// the next item. The last item is a value called `Nil`.
//
// Step 1: use a `Box` in the enum definition to make the code compile
// Step 2: create both empty and non-empty cons lists by replacing `todo!()`
//
// Note: the tests should not be changed
//
// Execute `rustlings hint box1` or use the `hint` watch subcommand for a hint.

// I AM NOT DONE

#[derive(PartialEq, Debug)]
pub enum List {
    Cons(i32, List),
    Nil,
}

fn main() {
    println!("This is an empty cons list: {:?}", create_empty_list());
    println!(
        "This is a non-empty cons list: {:?}",
        create_non_empty_list()
    );
}

pub fn create_empty_list() -> List {
    todo!()
}

pub fn create_non_empty_list() -> List {
    todo!()
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_create_empty_list() {
        assert_eq!(List::Nil, create_empty_list())
    }

    #[test]
    fn test_create_non_empty_list() {
        assert_ne!(create_empty_list(), create_non_empty_list())
    }
}

```

---

### 本题内容

本练习的目标是使用 Rust 的 `Box` 智能指针来构建递归数据结构。`Box` 允许在堆上存储数据，并且可以通过指针引用来处理递归类型，这在栈上是难以实现的因为栈需要在编译时知道数据的确切大小。在这个练习中，你将实现一个经典的函数式编程数据结构：cons list（列表构造）。

### 相关知识点

1. **递归类型和 `Box` 的使用**：
   - 递归类型是自身作为自己的一部分的数据类型。在 Rust 中，直接使用递归类型会因为无法确定其大小而导致编译错误。使用 `Box` 可以绕开这个问题，因为 `Box` 存储在堆上，并且只存储指向数据的指针。

2. **Cons List**：
   - Cons list 是一种构建自列表的传统方法，特别是在 Lisp 等函数式编程语言中常见。它通过一个头部（当前元素）和一个指向列表其余部分的指针（尾部）来构建。

3. **智能指针 `Box`**：
   - `Box<T>` 是一个智能指针，用于在堆上分配空间。除了允许处理递归类型外，`Box` 还可以用于处理大量数据，延长数据的生命周期，或拥有数据的所有权。

### 解题方法

#### 步骤描述：

**步骤 1**：修改 `List` 枚举，使用 `Box` 包装递归类型
- 在 `List` 枚举的 `Cons` 变体中，将第二个参数改为 `Box<List>` 类型，以使其可以递归。

**步骤 2**：实现创建空列表和非空列表的函数
- 创建空列表简单返回 `List::Nil`。
- 创建非空列表使用 `List::Cons` 构造，其中包含一个元素和一个指向另一个 `List`（可以是 `Nil` 或其他 `Cons`）的 `Box`。

#### 代码示例：

```rust
#[derive(PartialEq, Debug)]
pub enum List {
    Cons(i32, Box<List>),
    Nil,
}

fn main() {
    println!("This is an empty cons list: {:?}", create_empty_list());
    println!("This is a non-empty cons list: {:?}", create_non_empty_list());
}

pub fn create_empty_list() -> List {
    List::Nil
}

pub fn create_non_empty_list() -> List {
    List::Cons(1, Box::new(List::Cons(2, Box::new(List::Nil))))
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_create_empty_list() {
        assert_eq!(List::Nil, create_empty_list());
    }

    #[test]
    fn test_create_non_empty_list() {
        assert_ne!(create_empty_list(), create_non_empty_list());
    }
}
```

通过完成这个练习，你将理解如何在 Rust 中使用 `Box` 来创建复杂的递归数据结构，同时掌握 `Box` 在内存管理中的基本应用。这为处理更复杂的数据结构和深入学习 Rust 的内存模型奠定了基础。

## 扩展知识点与解答：

### 扩展知识点

1. **堆和栈内存分配**：
   - 在 Rust 中，栈内存用于存放已知固定大小的数据，而堆内存用于存放大小在编译时未知或可能会变化的数据。`Box` 是一个将数据存储在堆上的智能指针，帮助管理这类数据的生命周期和内存。

2. **智能指针类型**：
   - 除了 `Box`，Rust 还有其他类型的智能指针如 `Rc` 和 `Arc`，它们支持数据的多重所有权，这在并发编程中特别有用。了解不同智能指针的特性和使用场景可以帮助开发更安全、高效的应用。

3. **模式匹配与递归数据结构操作**：
   - 递归数据结构（如 cons list）常常与模式匹配结合使用，来递归访问或修改数据。这种模式在函数式编程中非常常见，可以有效地处理列表、树等结构。

### 扩展解题方法

1. **深入递归列表操作**：
   - 扩展现有的 `List` 实现，增加更多操作如 `append`、`reverse` 或 `map`。这些函数可以展示如何在递归数据结构上执行更复杂的操作。

2. **使用不同类型的数据**：
   - 修改 `List` 枚举，让它可以存储不仅仅是 `i32` 类型，而是任意泛型数据。这样可以增加列表的通用性和灵活性。

3. **错误处理和安全性**：
   - 在进行递归操作时添加错误处理逻辑，比如防止栈溢出或处理无效的递归操作。

#### 扩展代码示例：实现泛型和更多操作

```rust
#[derive(PartialEq, Debug)]
pub enum List<T> {
    Cons(T, Box<List<T>>),
    Nil,
}

impl<T> List<T> {
    pub fn new() -> Self {
        List::Nil
    }

    pub fn prepend(self, elem: T) -> List<T> {
        List::Cons(elem, Box::new(self))
    }

    pub fn reverse(self) -> List<T> {
        let mut result = List::new();
        let mut current = self;
        while let List::Cons(val, next) = current {
            result = result.prepend(val);
            current = *next;
        }
        result
    }

    pub fn map<U, F: Fn(T) -> U>(self, f: F) -> List<U> {
        match self {
            List::Cons(head, tail) => {
                List::Cons(f(head), Box::new(tail.map(f)))
            },
            List::Nil => List::Nil,
        }
    }
}

fn main() {
    let list = List::new().prepend(1).prepend(2).prepend(3);
    println!("List: {:?}", list);
    let reversed = list.reverse();
    println!("Reversed: {:?}", reversed);
    let mapped = reversed.map(|x| x * 2);
    println!("Mapped: {:?}", mapped);
}
```

通过这些扩展，你可以深入理解如何在 Rust 中使用递归数据结构和智能指针，同时掌握更高级的编程技巧，如泛型编程和函数式编程方法。这些技能对于构建复杂且高效的 Rust 应用程序是非常有价值的。

## `Box` 的背后机制及相关用法

`Box<T>` 在 Rust 中是一种非常基础的智能指针。它将数据存储在堆上，并通过栈上的指针进行访问。`Box` 通常用于以下几种情形：

1. **管理大型数据**：
   - 对于大型数据结构，将它们放在堆上而不是栈上可以防止栈溢出，并减少栈内存的消耗。这对性能有利，特别是在处理大量数据时。

2. **递归数据结构**：
   - 如前所述，`Box` 允许创建递归类型，例如链表和树等。这是因为 `Box` 的大小在编译时是已知的（即指针的大小），与其所指向的具体内容的大小无关。

3. **确保数据的唯一所有权**：
   - `Box` 提供了一种将数据从一个地方“移动”到另一个地方的方法，同时确保数据的唯一所有权。这在转移所有权时特别有用，例如在多线程编程中或者构建消费者-生产者模型时。

4. **面向对象编程（OOP）的特性**：
   - `Box` 可以用来构建包含多态行为的数据结构。通过 `Box` 可以持有不同类型但实现了相同 trait 的对象，这在 Rust 的面向对象编程中是处理动态分派的一种常用手段。

### 扩展用法

1. **性能优化**：
   - 当处理可变大小的大型集合时，使用 `Box` 可以减少栈上的内存压力。对于复杂的图结构或中间计算结果，使用 `Box` 可以提高缓存的有效性和管理内存的灵活性。

2. **函数和闭包**：
   - `Box` 可用于存储闭包和函数指针，尤其是那些占用空间较大或需要在多个上下文中传递的闭包。例如，在并发编程中，通过 `Box` 来持有并传递闭包给线程。

3. **接口与抽象化**：
   - 使用 `Box<dyn Trait>` 可以创建实现了某个 trait 的不同类型对象的集合。这在创建插件系统或需要运行时多态的场景下非常有用。

## 使用 `Box` 存储闭包

### 简单示例：

以下是一个使用 `Box` 存储闭包的示例，演示了如何将闭包作为函数参数动态传递：

```rust
fn execute_with_remark<F: FnOnce() -> String + 'static>(remark: Box<F>) {
    println!("Remark: {}", remark());
}

fn main() {
    let my_remark = Box::new(|| "Hello, Box!".to_string());
    execute_with_remark(my_remark);
}
```

### 扩展示例：

使用 `Box` 存储闭包在实际编程中尤其有用，尤其是当你需要在运行时决定哪些功能或行为将被执行时。以下示例演示了如何在一个结构体中使用 `Box` 存储闭包，并在用户输入的基础上动态改变行为。

#### 示例代码

在这个例子中，我们将创建一个 `TaskManager` 结构，这个结构可以存储和执行不同的任务，这些任务是通过闭包实现的。使用 `Box` 存储闭包允许我们在运行时动态地添加不同的任务。

```rust
fn main() {
    let mut manager = TaskManager::new();

    // 添加一个打印消息的任务
    manager.add_task(Box::new(|| {
        println!("Executing task 1: Printing a message.\n");
        "Task 1 completed".to_string()
    }));

    // 添加一个计算并返回结果的任务
    manager.add_task(Box::new(|| {
        let sum = (1..=10).sum::<i32>();
        println!("Executing task 2: Calculating the sum of 1 to 10.");
        format!("Task 2 result: {}", sum)
    }));

    // 执行所有任务并获取结果
    let results = manager.execute_all();
    for result in results {
        println!("{}", result);
    }
}

struct TaskManager {
    tasks: Vec<Box<dyn FnOnce() -> String>>,
}

impl TaskManager {
    fn new() -> Self {
        TaskManager { tasks: vec![] }
    }

    fn add_task(&mut self, task: Box<dyn FnOnce() -> String>) {
        self.tasks.push(task);
    }

    fn execute_all(&mut self) -> Vec<String> {
        let mut results = Vec::new();
        while let Some(task) = self.tasks.pop() {
            results.push(task());
        }
        results.reverse(); // 翻转结果以匹配添加顺序
        results
    }
}
```

#### 代码解析

1. **结构体定义**：
   - `TaskManager` 结构体包含一个 `tasks` 字段，这是一个动态闭包集合。这些闭包返回一个 `String` 类型的结果，并被定义为 `Box<dyn FnOnce() -> String>`。这意味着这些闭包只能被调用一次，这对于一次性任务是很常见的。

2. **任务管理**：
   - `add_task` 方法允许向任务列表中添加新的任务。任务以闭包形式提供，使用 `Box` 进行封装以存储在堆上。
   - `execute_all` 方法遍历并执行所有任务，收集它们的输出。使用 `FnOnce` 使得每个闭包在执行后被消耗，保证了每个任务只被执行一次。

3. **任务执行**：
   - 在主函数中，我们添加了两个不同的任务到 `TaskManager`。第一个任务打印一条消息，第二个任务执行一个计算并返回结果。
   - 执行任务并打印它们返回的结果。

这个示例展示了 `Box<dyn FnOnce()>` 的使用，这是处理运行时多态性和动态行为非常有力的工具，尤其适用于需要根据不同条件执行不同操作的场景。通过这种方式，Rust 程序可以在保持类型安全和效率的同时，获得极高的灵活性和表达力。
