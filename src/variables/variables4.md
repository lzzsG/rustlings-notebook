# Exercise 5

- Name: ```variables4```
- Path: ```exercises/variables/variables4.rs```
#### Hint: 

In Rust, variable bindings are immutable by default. 

But here we're trying to reassign a different value to x! 

There's a keyword we can use to make a variable binding mutable instead.


---



```rust,editable
// variables4.rs
//
// Execute `rustlings hint variables4` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

fn main() {
    let x = 3;
    println!("Number {}", x);
    x = 5; // don't change this line
    println!("Number {}", x);
}

```

---

### 本题内容：

本练习旨在向学生展示Rust中变量默认不可变性的特性，并引导学生学习如何通过使用`mut`关键字使变量变得可变。在Rust中，这种设计是为了增加代码的安全性和清晰性，但当你确实需要修改变量的值时，必须显式声明变量为可变。

### 相关知识点：

- **变量不可变性**：在Rust中，默认情况下，所有的变量都是不可变的。这意味着一旦变量被赋值，它的值就不能改变。这有助于预防意外修改数据的错误。
- **可变变量**：通过在变量声明时加上`mut`关键字，可以使变量可变。这表示你打算在变量的生命周期中修改它的值。

### 解题方法：

- **步骤描述**：
  - 要解决编译错误并按照题目要求修改变量`x`的值，需要在声明`x`时使其成为可变变量。这可以通过在`let`关键字后添加`mut`关键字来实现。
  
- **代码示例**：
    ```rust
    // variables4.rs
    // 已完成练习
    
    fn main() {
        let mut x = 3; // 使`x`变成可变的
        println!("Number {}", x);
        x = 5; // 现在可以改变`x`的值了
        println!("Number {}", x);
    }
    ```
    在这个修正后的版本中，通过添加`mut`关键字，`x`被声明为可变变量。这样，我们就可以在之后的代码中修改`x`的值，从3改为5。因此，程序首先打印"Number 3"，然后修改`x`的值并打印"Number 5"。

## 扩展知识点与解答：

深入探讨`variables4`练习后，我们可以扩展到关于Rust中变量可变性的更广泛概念，以及何时以及如何使用可变性来提高代码效率和可读性。

### 扩展知识点：

1. **内存安全和可变性**：
   - Rust的核心特性之一是其对内存安全的承诺，而变量的不可变性默认规则是这一承诺的关键部分。不可变性有助于防止数据竞争，这是并发编程中常见的问题。

2. **借用规则**：
   - Rust的借用规则（Borrowing rules）确保引用总是有效且不会导致数据竞争。在这些规则中，Rust区分了可变借用（`&mut`）和不可变借用（`&`），这直接关联到变量的可变性。

3. **当心过度使用`mut`**：
   - 虽然`mut`关键字在某些情况下是必要的，但过度使用可变变量可能会使代码难以理解和维护。良好的实践是默认使用不可变变量，只在需要修改变量值的时候使用`mut`。

### 扩展解题方法：

- **避免不必要的可变性**：
  在设计函数和算法时，应优先考虑使用不可变变量。如果一个变量在其生命周期内不需要改变，那么保持其不可变性可以提高代码的清晰度和安全性。

- **探索函数式编程模式**：
  Rust支持多种编程范式，包括函数式编程。在函数式编程中，不可变数据和无副作用的函数是核心概念。尝试使用这些模式可以减少对可变状态的依赖。

- **代码示例（利用作用域限制变量可变性）**：
    ```rust
    fn main() {
        let x = 3;
        println!("Number {}", x);
    
        {
            let mut x = x; // 通过创建一个新的作用域并重新绑定`x`为可变来修改值
            x = 5; 
            println!("Number {}", x);
        }
    
        println!("Number {}", x); // 原始的`x`保持不变
    }
    ```
    这个示例展示了如何通过限制可变性的作用域来保持代码的整洁和安全性。在一个新的作用域内，`x`被重新绑定为一个可变变量并被修改。这种方法允许我们在需要的时候使用可变性，同时限制其影响范围，保持代码的整体不可变性。
