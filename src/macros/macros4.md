# Exercise 87

- Name: ```macros4```
- Path: ```exercises/macros/macros4.rs```
#### Hint: 

You only need to add a single character to make this compile. The way macros are written, it wants to see something between each "macro arm", so it can separate them.

That's all the macro exercises we have in here, but it's barely even scratching the surface of what you can do with Rust's macros. For a more thorough introduction, you can have a read through the little book of Rust macros: [https://veykril.github.io/tlborm/](https://veykril.github.io/tlborm/) ([Chinese version](https://zjp-cn.github.io/tlborm/))


---



```rust,editable
// macros4.rs
//
// Execute `rustlings hint macros4` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

#[rustfmt::skip]
macro_rules! my_macro {
    () => {
        println!("Check out my macro!");
    }
    ($val:expr) => {
        println!("Look at this other macro: {}", $val);
    }
}

fn main() {
    my_macro!();
    my_macro!(7777);
}

```

---

### 本题内容

本练习旨在教授学习者如何正确地定义一个包含多个分支的宏（macro）。这是掌握 Rust 宏的重要步骤，因为它涉及到宏语法的具体细节，特别是如何区分宏的不同分支。

### 相关知识点

1. **宏分支**：
   - 宏可以有多个分支，每个分支都可以根据提供的输入模式匹配不同的调用。这类似于函数重载或模式匹配。

2. **分隔符**：
   - 宏分支之间通常使用分号（`;`）或逗号（`,`）作为分隔符。这些符号帮助编译器区分不同的宏规则。

3. **宏的表达式模式**：
   - `$val:expr` 是一个捕获表达式的宏模式，它可以匹配任何有效的 Rust 表达式。

### 解题方法

#### 步骤描述

1. **识别问题**：
   - 编译错误提示需要一个分隔符来区分宏的不同分支。

2. **修正宏定义**：
   - 在宏的第一个分支末尾添加逗号（`,`），以分隔两个分支。

3. **编译和测试**：
   - 修改后，重新编译代码以确保错误已解决。
   - 运行程序，检查输出是否符合预期，即打印两条不同的消息。

#### 代码示例

修正后的代码示例：

```rust
// macros4.rs

#[rustfmt::skip]
macro_rules! my_macro {
    () => {
        println!("Check out my macro!");
    }; // 添加分号来区分宏的分支
    ($val:expr) => {
        println!("Look at this other macro: {}", $val);
    }
}

fn main() {
    my_macro!();
    my_macro!(7777);
}
```

通过本练习，你将学习到 Rust 宏中的一个重要细节：如何使用分隔符来区分宏的多个分支。这是编写更复杂和功能丰富的宏时的基础知识，非常重要。理解和掌握这一点可以帮助你在需要时编写更加灵活和强大的宏，以应对不同的编程挑战。

## 扩展知识点与解答：

### 扩展知识点

1. **宏的扩展技术**：
   - 宏可以进行条件编译，根据编译时的条件来决定是否包含某些代码块。这常用于功能的可选编译和跨平台的代码支持。
   - 使用 `cfg!` 宏可以在宏内部检查编译条件，如操作系统或功能标志，以动态地改变宏的行为。

2. **宏的递归能力**：
   - 宏可以递归地调用自身，这允许在处理列表、树或其他递归数据结构时进行复杂的模式匹配和代码生成。
   - 递归宏需要特别注意其终止条件，以防止编译时无限递归。

3. **过程宏**：
   - 过程宏提供了更强大的代码生成能力，它们可以操作整个语法树，生成或修改任意 Rust 代码。
   - 过程宏分为三类：派生宏（Derive macros）、属性宏（Attribute macros）、函数宏（Function-like macros），每种类型都用于不同的用途。

### 扩展解题方法

1. **宏的模块化和重用**：
   - 宏的模块化是保持代码清晰和可维护的关键。通过将宏定义在专用的模块或库中，并使用 `#[macro_export]` 属性导出宏，可以在多个项目中重用这些宏。
   - 为宏创建文档注释和示例代码，这不仅有助于他人理解和使用你的宏，还可以通过文档测试确保宏的正确性。

2. **测试宏的正确性和效率**：
   - 对宏进行彻底的测试，尤其是在它们用于关键业务逻辑时。测试应覆盖各种输入场景，以确保宏在不同条件下都能正确展开。
   - 分析宏展开后的代码性能，确保它不会无意中引入性能瓶颈或增大编译时间。

3. **宏的错误处理和调试**：
   - 宏的错误处理需要特别小心，因为错误信息有时可能难以理解。在开发宏时，使用清晰的错误消息和尽可能多的帮助信息是很有帮助的。
   - 调试宏时，利用 `cargo expand` 等工具查看宏展开的具体代码，这可以帮助理解宏在特定情况下的行为。

通过这些扩展知识点和解题方法，你可以更深入地掌握 Rust 宏的高级应用，并能够在你的项目中有效地利用宏来编写更高效、更强大的代码。
