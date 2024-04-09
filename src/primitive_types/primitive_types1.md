# Exercise 17

- Name: ```primitive_types1```
- Path: ```exercises/primitive_types/primitive_types1.rs```
#### Hint: 

No hints this time ;)


---



```rust,editable
// primitive_types1.rs
//
// Fill in the rest of the line that has code missing! No hints, there's no
// tricks, just get used to typing these :)
//
// Execute `rustlings hint primitive_types1` or use the `hint` watch subcommand
// for a hint.

// I AM NOT DONE

fn main() {
    // Booleans (`bool`)

    let is_morning = true;
    if is_morning {
        println!("Good morning!");
    }

    let // Finish the rest of this line like the example! Or make it be false!
    if is_evening {
        println!("Good evening!");
    }
}

```

---

### 本题内容：

这个练习旨在让学生熟悉Rust中的布尔类型(`bool`)，通过完成一个简单的条件判断任务来加深对布尔值的理解。练习要求补全缺失的代码，正确声明一个布尔变量，并使用它在不同条件下打印出相应的问候语。

### 相关知识点：

- **布尔类型(`bool`)**：Rust中的布尔类型用于表示逻辑值，可以是`true`或`false`。这是最基本的数据类型之一，广泛用于条件判断和循环控制。
- **条件语句**：Rust中的`if`语句用于基于布尔表达式的结果来执行不同的代码分支。这是控制流的基本结构之一。

### 解题方法：

- **步骤描述**：
  1. 声明一个名为`is_evening`的变量，并将其设置为布尔值`true`或`false`。
  2. 使用这个变量作为`if`语句的条件，根据其值打印出不同的消息。

- **代码示例**：
    

```rust
// primitive_types1.rs
// 已完成练习

fn main() {
    // Booleans (`bool`)
    let is_morning = true;
    if is_morning {
        println!("Good morning!");
    }

    // 补全布尔变量的声明
    let is_evening = false; // 或者将其设置为true，根据个人喜好
    if is_evening {
        println!("Good evening!");
    }
}
```
在这个修正后的版本中，我们添加了一个名为`is_evening`的布尔变量，并根据其值在条件语句中选择不同的分支。如果你将`is_evening`设置为`true`，程序将打印"Good evening!"；如果为`false`，则不会打印任何晚上的问候。

通过解决这个练习，你将对Rust中的布尔类型有了初步的了解，并学会了如何使用条件语句根据布尔值控制程序的执行流程。这是学习任何编程语言中的基本技能之一。

## 扩展知识点与解答：

探索`primitive_types1`练习之后，我们可以进一步深入Rust中基本数据类型的使用及其在实际编程中的应用。现在，让我们扩展一些相关的知识点和编程技巧。

### 扩展知识点：

1. **基本数据类型**：
   - 除了布尔类型(`bool`)外，Rust还提供了一系列的基本数据类型，包括整数(`i32`, `u32`, `i64`, `u64`等)、浮点数(`f32`, `f64`)、字符(`char`)和元组等。每种类型都有其特定的用途和表达范围。
3. **数据类型转换**：
   - 在处理不同类型的数据时，有时需要进行类型转换。Rust中的类型转换需要显式进行，使用`as`关键字或通过调用特定的转换函数，如`.to_string()`方法将数字转换为字符串。

### 扩展解题方法：

- **探索其他基本类型**：
  - 尝试声明和使用Rust中的其他基本类型变量，例如整数、浮点数和字符，来加深对这些类型的理解。

- **练习数据类型转换**：
  - 编写一段代码，涉及到基本数据类型之间的转换，例如将整数转换为浮点数，或将数字转换为字符串。

- **代码示例（变量声明和类型转换）**：
    

```rust
fn main() {
    let an_integer: u32 = 42;
    let a_float: f64 = an_integer as f64 + 0.5;
    let a_string = an_integer.to_string();

    println!("Integer: {}", an_integer);
    println!("Float: {}", a_float);
    println!("String: {}", a_string);
}
```
这段代码展示了如何在Rust中声明不同类型的变量，并进行类型转换。首先，我们声明了一个无符号整数`an_integer`，然后将其转换为浮点数`a_float`，最后将整数转换为字符串`a_string`并打印出来。

通过掌握这些扩展知识点和解题方法，你将能够更深入地理解Rust的基本数据类型及其应用，为后续学习Rust的更高级特性打下坚实的基础。
