# Exercise 47

- Name: ```quiz2```
- Path: ```exercises/quiz2.rs```
#### Hint: 

No hints this time ;)


---



```rust,editable
// quiz2.rs
//
// This is a quiz for the following sections:
// - Strings
// - Vecs
// - Move semantics
// - Modules
// - Enums
//
// Let's build a little machine in the form of a function. As input, we're going
// to give a list of strings and commands. These commands determine what action
// is going to be applied to the string. It can either be:
// - Uppercase the string
// - Trim the string
// - Append "bar" to the string a specified amount of times
// The exact form of this will be:
// - The input is going to be a Vector of a 2-length tuple,
//   the first element is the string, the second one is the command.
// - The output element is going to be a Vector of strings.
//
// No hints this time!

// I AM NOT DONE

pub enum Command {
    Uppercase,
    Trim,
    Append(usize),
}

mod my_module {
    use super::Command;

    // TODO: Complete the function signature!
    pub fn transformer(input: ???) -> ??? {
        // TODO: Complete the output declaration!
        let mut output: ??? = vec![];
        for (string, command) in input.iter() {
            // TODO: Complete the function body. You can do it!
        }
        output
    }
}

#[cfg(test)]
mod tests {
    // TODO: What do we need to import to have `transformer` in scope?
    use ???;
    use super::Command;

    #[test]
    fn it_works() {
        let output = transformer(vec![
            ("hello".into(), Command::Uppercase),
            (" all roads lead to rome! ".into(), Command::Trim),
            ("foo".into(), Command::Append(1)),
            ("bar".into(), Command::Append(5)),
        ]);
        assert_eq!(output[0], "HELLO");
        assert_eq!(output[1], "all roads lead to rome!");
        assert_eq!(output[2], "foobar");
        assert_eq!(output[3], "barbarbarbarbarbar");
    }
}

```

---

这个题目是一个小测验，涵盖了字符串、向量（Vecs）、移动语义、模块和枚举等几个部分。我们要创建一个小型机器（一个函数），它会根据指定的命令来处理一系列字符串。命令包括将字符串转换为大写（Uppercase）、修剪字符串（Trim）或者指定次数地追加"bar"（Append）。下面是如何逐步解决这个练习的方法。

### 本题内容：

- 输入：一个元组的向量，元组的第一部分是字符串，第二部分是命令（枚举Command）。
- 输出：字符串的向量，这些字符串已根据命令进行了相应的转换。

### 相关知识点：

- **枚举（Enums）**：用于定义命令的类型，包括Uppercase、Trim和Append(usize)。
- **模块（Modules）**：将转换器函数放在一个模块中，需要正确导入使用。
- **字符串（Strings）和向量（Vecs）**：输入和输出的数据结构。
- **移动语义（Move Semantics）**：理解函数如何消费或借用输入参数。

### 解题方法：

### 定义函数签名

首先，需要定义`transformer`函数的签名。输入是元组的向量，元组包含`String`和`Command`。输出是`String`的向量。

```rust
pub fn transformer(input: Vec<(String, Command)>) -> Vec<String> {
```

### 完成输出声明

输出是`String`的向量：

```rust
let mut output: Vec<String> = vec![];
```

### 实现功能

遍历输入向量，根据命令对字符串进行相应的处理，并将结果添加到输出向量中。

```rust
for (mut string, command) in input.iter() {
    match command {
        Command::Uppercase => output.push(string.to_uppercase()),
        Command::Trim => output.push(string.trim().to_string()),
        Command::Append(n) => {
            let mut new_string = string.clone();
            for _ in 0..*n {
                new_string.push_str("bar");
            }
            output.push(new_string);
        },
    }
}
```

在这段 Rust 代码中，我们处理一个元组的迭代器 `input`，每个元组包含一个字符串和一个 `Command` 枚举类型的命令。对于每个元组，根据 `Command` 枚举的值对字符串进行相应的操作，并将结果字符串推送到输出向量 `output` 中。下面是详细解释：

**输入与命令枚举**

- `input` 是一个包含元组的迭代器，其中每个元组由一个字符串和一个 `Command` 枚举值组成。
- `Command` 是一个枚举，定义了可以应用于字符串的不同命令，如 `Uppercase`、`Trim` 和 `Append(n)`。

**循环与匹配**

- `for (mut string, command) in input.iter()` 这一行设置了一个循环，遍历 `input` 中的每个元组。`string` 是元组中的字符串，`command` 是应用于该字符串的命令。

**匹配操作**

- `match command` 结构用于根据 `command` 的值选择执行不同的操作。对于每种命令，我们如下操作：

  1. **Uppercase**：
     - 如果命令是 `Uppercase`，则调用 `string.to_uppercase()` 将字符串转换为大写，并将转换后的字符串推送到输出向量 `output` 中。

  2. **Trim**：
     - 如果命令是 `Trim`，则调用 `string.trim()` 去除字符串首尾的空白字符，并将结果转换为一个新的 `String` 对象，然后推送到 `output`。

  3. **Append(n)**：
     - 如果命令是 `Append(n)`，其中 `n` 是一个整数，表示需要重复追加的次数（例如，追加 "bar" 字符串 `n` 次到原始字符串后）。首先克隆原始字符串到 `new_string`，然后在一个循环中追加 "bar" `n` 次。完成后，将新的字符串推送到 `output`。

**输出向量**

- `output` 是一个向量，用于存储每次迭代修改后的字符串。

### 导入模块

在测试模块中，需要导入`transformer`函数。如果`transformer`函数位于`my_module`模块中，应这样导入：

```rust
use crate::my_module::transformer;
```

完成以上步骤后，你的函数就应该能正确处理输入，并在测试中返回预期的输出了。这个练习综合了你之前学过的几个主题，是对你知识的一个全面测试。

## 扩展知识点与解答：

这个练习结合了枚举、模块、向量、字符串处理和移动语义等多个概念，提供了一个很好的综合练习机会。以下是扩展的知识点和解答方法：

### 扩展知识点：

- **枚举的高级用法**：枚举不仅可以用作单纯的标签（如`Uppercase`），还可以携带数据（如`Append(usize)`）。这种用法极大地增强了枚举的表达能力，使其能够在一个统一的类型中表示多种相关但形式不同的数据。

- **迭代器和闭包**：在处理向量和字符串时，迭代器（Iterator）和闭包（Closure）是处理集合数据的强大工具。例如，使用`.iter()`遍历输入向量，或者使用`.map()`、`.filter()`等迭代器适配器进行转换和筛选。

- **模块和可见性**：理解公有（`pub`）和私有（默认）可见性，以及如何在模块间导入和使用函数、类型和常量，是组织大型Rust项目的基础。

### 扩展解答方法：

1. **使用模式匹配（Pattern Matching）**：枚举`Command`的处理中，利用`match`表达式可以优雅地根据不同的命令类型执行不同的操作。这显示了Rust中模式匹配的强大能力，尤其是在处理枚举类型时。

2. **借用与所有权（Borrowing and Ownership）**：函数接收一个向量的引用并返回一个新的向量，这里需要注意的是，对于`Append`命令，由于字符串`String`是不可复制的类型，所以需要先克隆原始字符串再进行操作。这涉及到Rust的所有权和借用规则，特别是当需要修改数据时如何正确地管理所有权。

3. **使用`or_insert`方法处理HashMap**：虽然这个练习没有直接使用`HashMap`，但在处理类似“如果存在则更新，否则插入”这样的逻辑时，`HashMap`的`entry`方法和`or_insert`方法可以非常高效地处理。这种模式在处理需要统计或者聚合数据时特别有用。

4. **模块的使用（Using Modules）**：将功能逻辑封装在模块中，不仅可以提高代码的组织性和可读性，还能有效地管理命名空间和可见性。在测试模块中正确导入使用的函数是理解Rust模块系统的重要一步。

5. **错误处理**：在处理`String`转换和解析时，适当的错误处理可以使程序更加健壮。虽然在这个简单的练习中可能直接使用`unwrap`来处理结果，但在实际应用中，合理使用`Result`类型和错误处理是非常重要的编程实践。

通过这个测验，你可以检验自己对这些概念的理解和应用能力，同时也是对之前学习内容的复习和巩固。
