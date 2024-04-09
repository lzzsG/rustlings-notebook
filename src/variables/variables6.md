# Exercise 7

- Name: ```variables6```
- Path: ```exercises/variables/variables6.rs```
#### Hint: 

We know about variables and mutability, but there is another important type of variable available: constants.

Constants are always immutable and they are declared with keyword 'const' rather than keyword 'let'. Constants types must also always be annotated.

Read more about constants and the differences between variables and constants under 'Constants' in the book's section 'Variables and Mutability':

[https://doc.rust-lang.org/book/ch03-01-variables-and-mutability.html#constants](https://doc.rust-lang.org/book/ch03-01-variables-and-mutability.html#constants)

[Chinese version](https://rustwiki.org/zh-CN/book/ch03-01-variables-and-mutability.html#%E5%B8%B8%E9%87%8F)

---



```rust,editable
// variables6.rs
//
// Execute `rustlings hint variables6` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

const NUMBER = 3;
fn main() {
    println!("Number {}", NUMBER);
}

```

---

### 本题内容：

此练习介绍了Rust中的另一种变量类型：常量。常量是一种总是不可变的特殊变量，它们使用`const`关键字而不是`let`关键字来声明。常量的使用场景包括存储程序中不会改变的值，例如配置值或数学常数等。

### 相关知识点：

- **常量**：在Rust中，常量使用`const`关键字声明，并且常量总是不可变的。常量的类型必须显式指定。
- **不变性**：与使用`let`声明的不可变变量不同，常量不仅值不可变，而且在编译时就必须已知，且它们有全局作用域。
- **类型注解**：声明常量时必须显式提供类型注解，因为常量在编译时就需要确定其类型。

### 解题方法：

- **步骤描述**：
  - 要解决这个练习的问题，需要确保常量`NUMBER`的声明遵循Rust的常量声明规则。具体来说，需要显式注明常量的类型。
  
- **代码示例**：
    ```rust
    // variables6.rs
    // 已完成练习
    
    const NUMBER: i32 = 3; // 为常量`NUMBER`显式注明类型`i32`
    fn main() {
        println!("Number {}", NUMBER);
    }
    ```
    在这个修正后的版本中，常量`NUMBER`在声明时被显式指定为`i32`类型。这符合Rust中常量声明的要求，即常量必须有明确的类型注解。因此，这段代码将成功编译，并在运行时打印出"Number 3"。

## 扩展知识点与解答：

探索`variables6`练习后，我们可以进一步理解Rust中常量与变量之间的区别，以及常量在实际编程中的用途和优势。

### 扩展知识点：

1. **常量与变量的区别**：
   - **作用域**：常量可以在全局作用域中声明，使得它们在整个程序中都可用。相比之下，变量的作用域通常限制在声明它们的块内。
   - **内存效率**：常量在编译时被评估，这意味着它们的使用可能比运行时分配的变量更内存效率。
   - **性能考虑**：使用常量可以帮助Rust编译器做出更优化的决策，因为它们的值在编译时已知且不可改变。

2. **使用场景**：
   - 常量经常用于定义程序运行过程中不会改变的值，如数学常数、环境配置参数等。

3. **命名规范**：
   - Rust的习惯是使用全大写字母和下划线分隔来命名常量，例如`CONSTANT_VALUE`。这有助于在代码中清楚地区分常量和变量。

### 扩展解题方法：

- **全局常量**：
  - 考虑将常用的不变值声明为全局常量，这样可以在程序的多个部分中复用这些值，而不需要重复定义。

- **性能优化**：
  - 在性能敏感的应用中，优先考虑使用常量代替运行时计算的值，特别是在循环或频繁调用的函数中。

- **代码示例（使用全局常量）**：
    ```rust
    const MAX_POINTS: u32 = 100_000;
    
    fn main() {
        let mut points = 50_000;
        if points > MAX_POINTS {
            println!("You've reached the maximum number of points!");
        } else {
            points += 30; // 假设增加一些点数
            println!("Points have been increased to {}", points);
        }
    }
    ```
    在这个示例中，`MAX_POINTS`常量在全局作用域中声明，表示程序中的最大点数。这个常量在`main`函数中用于比较和决策，展示了如何在实际编程中使用常量来管理和比较固定的值。
