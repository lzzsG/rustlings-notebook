# Exercise 9

- Name: ```functions2```
- Path: ```exercises/functions/functions2.rs```
#### Hint: 

Rust requires that all parts of a function's signature have type annotations, but `call_me` is missing the type annotation of `num`.


---



```rust,editable
// functions2.rs
//
// Execute `rustlings hint functions2` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

fn main() {
    call_me(3);
}

fn call_me(num:) {
    for i in 0..num {
        println!("Ring! Call number {}", i + 1);
    }
}

```

---

### 本题内容：

这个练习目的是让学生实践Rust中函数参数类型注解的要求。在Rust中，所有函数参数都必须有明确的类型注解，以便编译器进行类型检查和推导。本题中的挑战在于修复`call_me`函数的定义，为缺失的参数`num`添加正确的类型注解。

### 相关知识点：

- **类型注解**：在Rust中，函数定义时参数需要明确指定类型，这有助于编译器检查类型错误，保证代码安全性。
- **循环控制**：本练习还涉及使用`for`循环遍历一定范围内的数字，这是控制流的一种基本形式。

### 解题方法：

- **步骤描述**：
  - 根据题目要求和Rust的规则，我们需要为`call_me`函数中的参数`num`添加一个类型注解。考虑到`num`被用作`for`循环的范围上限，合理的类型注解应该是某种整数类型。Rust中最常用的整数类型是`i32`。

- **代码示例**：
    

```rust
// functions2.rs
// 已完成练习

fn main() {
    call_me(3);
}

// 为`num`参数添加了类型注解`i32`
fn call_me(num: i32) {
    for i in 0..num {
        println!("Ring! Call number {}", i + 1);
    }
}
```
在这个修正后的版本中，我们为`call_me`函数的`num`参数添加了`: i32`类型注解，明确了它应该是一个32位整数。这样就满足了Rust函数签名对于类型注解的要求，代码现在可以正确编译和运行，每次调用`call_me(3);`将会打印三次"Ring! Call number X"，其中X是从1开始的呼叫次数。

## 扩展知识点与解答：

深入探讨`functions2`练习之后，我们可以扩展到Rust函数的更多高级特性和用法，包括参数类型的多样性、函数重载的替代方案，以及利用泛型进行函数定义。

### 扩展知识点：

1. **参数类型多样性**：
   - Rust支持多种数据类型作为函数参数，包括基本数据类型、自定义数据类型、引用和复杂的数据结构等。理解如何根据需要选择合适的参数类型是编写高效Rust代码的关键。

2. **函数重载**：
   - 与某些其他语言不同，Rust不支持传统意义上的函数重载（即同名函数根据不同的参数类型或数量进行区分）。然而，可以通过使用泛型或特征（traits）来实现类似功能，或简单地为不同的功能使用不同的函数名。

3. **泛型函数**：
   - 泛型允许在定义函数（或数据类型）时使用类型参数，从而使函数能够更灵活地处理不同类型的参数。泛型的使用可以增加代码的复用性和灵活性。

### 扩展解题方法：

- **探索泛型函数**：
  - 尝试定义一个泛型函数，它可以接受任意类型的参数，并根据参数执行操作。这有助于理解泛型如何提高函数的通用性和复用性。

- **代码示例（使用泛型和`Display`特征）**：
    

```rust
use std::fmt::Display;

fn print_me<T: Display>(item: T) {
    println!("Printing: {}", item);
}

fn main() {
    print_me(3);         // 打印一个整数
    print_me("Hello");  // 打印一个字符串
}
```
在这个示例中，`print_me`函数使用泛型`T`，它要求`T`实现了`Display`特征，这使得几乎任何可打印的类型都可以作为其参数。这显示了如何使用泛型来创建可接受多种类型参数的函数。
