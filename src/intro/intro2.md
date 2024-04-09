# Exercise 1

- Name: ```intro2```
- Path: ```exercises/intro/intro2.rs```

#### Hint

Add an argument after the format string.

---
```rust,editable
// intro2.rs
//
// Make the code print a greeting to the world.
//
// Execute `rustlings hint intro2` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

fn main() {
    println!("Hello {}!");
}

```

---

### 本题内容：

这个练习目的是教会学生如何使用Rust的宏`println!`来打印带有变量的字符串。Rust的`println!`宏允许你在字符串中插入变量，这对于生成动态内容的输出非常有用。通过完成这个练习，学生将学习如何将变量嵌入到字符串中，并让代码打印出一个向世界问好的问候语。

### 相关知识点：

- **宏**：Rust的宏提供了一种用于元编程的强大工具。`println!`是最常用的宏之一，用于向标准输出打印信息。
- **字符串格式化**：在Rust中，`{}`用作占位符，用于在字符串中插入变量的值。
- **变量传递**：向`println!`宏传递额外的参数来替换字符串中的占位符。

### 解题方法：

- **步骤描述**：为了完成这个练习，我们需要向`println!`宏的字符串参数后添加一个实际的参数，以替换字符串中的`{}`占位符。给定的代码意图是打印一条问候语，所以我们可以传递一个表示"world"或其他任何对象的字符串字面值作为参数。

- **代码示例**：
    ```rust
    // intro2.rs
    // 已完成练习
    
    fn main() {
        // 在字符串"Hello {}!"中的占位符{}后添加了一个实际的参数"world"
        println!("Hello {}!", "world");
    }
    ```
    在这个修正后的版本中，我们向`println!`宏添加了`"world"`作为参数，这样它就会在打印时替换字符串中的`{}`。执行后，终端将显示消息"Hello world!"，成功完成了向世界问好的任务。



## 扩展知识点与解答：

在完成`intro2`这个练习的基础上，我们可以进一步探索Rust中的字符串格式化、宏使用以及如何构建更复杂的字符串消息。

### 扩展知识点：

1. **字符串格式化高级用法**：
   - Rust通过`format!`宏提供了丰富的字符串格式化功能，类似于`println!`，但它返回的是一个格式化后的字符串而不是直接打印。
   - 除了基本的占位符`{}`之外，你还可以在占位符中指定变量的位置、使用命名参数、控制数字的格式化显示（比如：十六进制、二进制表示）、控制宽度和对齐等。

2. **复杂的宏使用**：
   - Rust的宏系统非常强大，能够处理各种复杂的情况，比如条件编译、生成代码等。了解如何自定义宏可以极大地提升你的Rust编程技能。

3. **字符串的不可变性与所有权**：
   - 在Rust中，字符串是不可变的，除非你使用可变引用或特定的字符串类型，如`String`。理解Rust的所有权系统对于有效地管理内存和防止数据竞争至关重要。

### 扩展解题方法：

- **使用命名参数**：
  Rust的格式化字符串支持命名参数，这使得复杂字符串的构建更加直观和易于管理。

- **代码示例（命名参数）**：
    ```rust
    fn main() {
        let target = "world";
        println!("Hello {target}!");
    }
    ```
  在这个示例中，我们使用了一个命名参数`target`来替代占位符。这种方式让代码更加清晰，尤其是在字符串格式化中有多个参数需要替换时。

- **条件编译日志消息**：
  在开发中，你可能希望在调试版本中打印额外的日志信息，而在发布版本中省略这些信息。Rust通过条件编译提供了这种灵活性。

- **代码示例（条件编译）**：
    ```rust
    fn main() {
        println!("Hello, world!");
        
        #[cfg(debug_assertions)]
        println!("This message is visible in debug mode only.");
    }
    ```
  使用`#[cfg(debug_assertions)]`属性，上述的第二条`println!`仅在编译调试版本时执行，这对于调试非常有用。
