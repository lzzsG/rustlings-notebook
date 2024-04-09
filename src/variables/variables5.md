# Exercise 6

- Name: ```variables5```
- Path: ```exercises/variables/variables5.rs```
#### Hint: 

In variables4 we already learned how to make an immutable variable mutableusing a special keyword. Unfortunately this doesn't help us much in this exercise because we want to assign a different typed value to an existing variable.

Sometimes you may also like to reuse existing variable names because you are just converting
values to different types like in this exercise.

Fortunately Rust has a powerful solution to this problem: 'Shadowing'!You can read more about 'Shadowing' in the book's section 'Variables and Mutability':

[https://doc.rust-lang.org/book/ch03-01-variables-and-mutability.html#shadowing](https://doc.rust-lang.org/book/ch03-01-variables-and-mutability.html#shadowing)

[Chinese version](https://rustwiki.org/zh-CN/book/ch03-01-variables-and-mutability.html#%E9%81%AE%E8%94%BD)

Try to solve this exercise afterwards using this technique.


---



```rust,editable
// variables5.rs
//
// Execute `rustlings hint variables5` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

fn main() {
    let number = "T-H-R-E-E"; // don't change this line
    println!("Spell a Number : {}", number);
    number = 3; // don't rename this variable
    println!("Number plus two is : {}", number + 2);
}

```

---

### 本题内容：

这个练习目的是向学生展示Rust中变量遮蔽（Shadowing）的概念及其用途。通过变量遮蔽，你可以在相同作用域内用新值重新声明一个新的同名变量，甚至允许这个新的变量有一个不同的类型。这在需要转换值类型或者简单地重用变量名时非常有用。

### 相关知识点：

- **变量遮蔽**：Rust允许在相同的作用域内用新的值重新声明一个同名变量，这个过程被称为变量遮蔽。这与可变变量不同，因为它实际上创建了一个新的变量。
- **类型转换与重用变量名**：变量遮蔽允许改变变量的类型，这对于在处理数据转换或者临时改变变量类型的场景下非常有用。

### 解题方法：

- **步骤描述**：
  - 要解决这个练习，我们需要利用变量遮蔽的特性来“重新声明”变量`number`，使其能够在第二次`println!`调用中作为一个整数类型使用。这意味着我们将在尝试给`number`赋予一个整数值之前，使用`let`关键字来遮蔽前一个`number`变量。

- **代码示例**：
    ```rust
    // variables5.rs
    // 已完成练习
    
    fn main() {
        let number = "T-H-R-E-E"; // 不改变这行
        println!("Spell a Number : {}", number);
        let number = 3; // 利用变量遮蔽来重新声明`number`为整数
        println!("Number plus two is : {}", number + 2);
    }
    ```
    在这个修正后的版本中，我们通过重新声明`number`变量来遮蔽之前的字符串类型的`number`变量。新的`number`变量是一个整数类型，这使得我们可以在计算`number + 2`时不会遇到类型不匹配的错误。因此，第一次`println!`调用打印出原始的字符串值，而第二次调用则打印出整数5。

## 扩展知识点与解答：

通过深入探讨`variables5`练习，我们可以扩展到关于变量遮蔽在Rust中的使用场景、优势、以及与可变性的比较等更广泛的知识。

### 扩展知识点：

1. **变量遮蔽 vs 可变性**：
   - 变量遮蔽允许你在相同的名称下重新绑定一个变量，甚至可以改变其类型。这与可变变量不同，可变变量要求类型保持不变，仅值可以改变。
   - 遮蔽通过重新声明变量实现，而不是修改变量的值。这意味着遮蔽的变量仍然遵循Rust的不可变性规则。

2. **类型转换**：
   - 在使用变量遮蔽进行类型转换时，它提供了一种在处理变量时临时改变类型的便捷方式，特别是在进行数据解析或转换时。

3. **内存管理**：
   - 变量遮蔽对于内存管理来说是高效的，因为它允许在不同阶段使用不同类型的数据，而无需为每种类型分配单独的变量。

### 扩展解题方法：

- **合理使用变量遮蔽**：
  - 在需要对同一个变量名执行不同类型的操作时合理使用变量遮蔽，比如当变量从一个类型转换为另一个类型，或者当你想重新使用某个变量名表示不同的数据时。

- **代码示例（变量遮蔽与类型转换）**：
    ```rust
    fn main() {
        let text = "42"; // 初始为字符串
        let text = text.parse::<i32>().expect("Not a number!"); // 遮蔽为i32类型
        // 现在text是整数类型
        println!("Number is: {}", text);
    }
    ```
    这个示例演示了如何将一个字符串类型的变量通过变量遮蔽转换为整数类型。首先，`text`被声明为一个字符串。然后，使用相同的变量名`text`，通过`parse`方法尝试将字符串转换为`i32`类型，成功转换后遮蔽原来的变量。
