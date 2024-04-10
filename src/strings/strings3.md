# Exercise 39

- Name: ```strings3```
- Path: ```exercises/strings/strings3.rs```
#### Hint: 

There's tons of useful standard library functions for strings. Let's try and use some of
them: [https://doc.rust-lang.org/std/string/struct.String.html#method.trim](https://doc.rust-lang.org/std/string/struct.String.html#method.trim) ([Chinese version](https://www.rustwiki.org.cn/zh-CN/std/primitive.str.html#method.trim))!



For the compose_me method: You can either use the `format!` macro, or convert the string
slice into an owned string, which you can then freely extend.


---



```rust,editable
// strings3.rs
//
// Execute `rustlings hint strings3` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

fn trim_me(input: &str) -> String {
    // TODO: Remove whitespace from both ends of a string!
    ???
}

fn compose_me(input: &str) -> String {
    // TODO: Add " world!" to the string! There's multiple ways to do this!
    ???
}

fn replace_me(input: &str) -> String {
    // TODO: Replace "cars" in the string with "balloons"!
    ???
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn trim_a_string() {
        assert_eq!(trim_me("Hello!     "), "Hello!");
        assert_eq!(trim_me("  What's up!"), "What's up!");
        assert_eq!(trim_me("   Hola!  "), "Hola!");
    }

    #[test]
    fn compose_a_string() {
        assert_eq!(compose_me("Hello"), "Hello world!");
        assert_eq!(compose_me("Goodbye"), "Goodbye world!");
    }

    #[test]
    fn replace_a_string() {
        assert_eq!(replace_me("I think cars are cool"), "I think balloons are cool");
        assert_eq!(replace_me("I love to look at cars"), "I love to look at balloons");
    }
}

```

---

### 本题内容

本练习旨在通过对字符串的处理操作，如去除空白、字符串拼接和替换特定子字符串，来深化对Rust中`String`类型方法和功能的理解。

### 相关知识点

- **去除字符串空白**：`trim`方法可以去除字符串首尾的空白字符，包括空格、制表符、换行符等。
- **字符串拼接**：可以使用`+`运算符或`format!`宏来拼接字符串。`+`运算符要求右侧必须是字符串字面量的引用`&str`，而`format!`宏更灵活，可以接受多种类型的参数。
- **替换字符串中的子字符串**：`replace`方法允许你在一个字符串中替换所有匹配的子字符串。

### 解题方法

#### 去除空白

使用`trim`方法去除输入字符串首尾的空白字符。

```rust
fn trim_me(input: &str) -> String {
    input.trim().to_string()
}
```

#### 字符串拼接

这里我们展示使用`format!`宏的方法，因为它提供了更多的灵活性。

```rust
fn compose_me(input: &str) -> String {
    format!("{} world!", input)
}
```

#### 替换子字符串

使用`replace`方法在字符串中替换所有出现的特定子字符串。

```rust
fn replace_me(input: &str) -> String {
    input.replace("cars", "balloons")
}
```

### 完整代码示例

将之前的解题方法整合到完整代码中：

```rust
fn trim_me(input: &str) -> String {
    input.trim().to_string()
}

fn compose_me(input: &str) -> String {
    format!("{} world!", input)
}

fn replace_me(input: &str) -> String {
    input.replace("cars", "balloons")
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn trim_a_string() {
        assert_eq!(trim_me("Hello!     "), "Hello!");
        assert_eq!(trim_me("  What's up!"), "What's up!");
        assert_eq!(trim_me("   Hola!  "), "Hola!");
    }

    #[test]
    fn compose_a_string() {
        assert_eq!(compose_me("Hello"), "Hello world!");
        assert_eq!(compose_me("Goodbye"), "Goodbye world!");
    }

    #[test]
    fn replace_a_string() {
        assert_eq!(replace_me("I think cars are cool"), "I think balloons are cool");
        assert_eq!(replace_me("I love to look at cars"), "I love to look at balloons");
    }
}
```

通过这些例子，你不仅解决了具体的编程问题，还学会了一些处理Rust字符串的常用方法。这些技能对于编写处理字符串数据的Rust程序非常重要。

## 扩展知识点与解答：

### 扩展知识点

掌握了`strings3`练习的基础后，我们可以进一步探索Rust中与字符串处理相关的更多高级概念和技术。以下是一些扩展的知识点：

1. **字符串切片和索引**：由于Rust字符串是UTF-8编码的，直接通过索引访问可能不总是直观的，因为一个Unicode字符可能由多个字节组成。理解如何安全地使用字符串切片和字符边界非常重要。
2. **字符串迭代**：Rust允许你通过`.chars()`和`.bytes()`方法迭代字符串的字符或字节。这对于需要逐字符或逐字节处理字符串数据的情况非常有用。
3. **字符串和其他类型之间的转换**：了解如何将字符串转换为其他类型（如整数或浮点数）以及如何从其他类型转换回字符串是常见的需求。`parse`方法和`to_string`方法或`format!`宏在这里发挥作用。

### 扩展解题方法

在`strings3`的基础上，我们可以利用上述知识点，探索一些与字符串操作相关的扩展解题方法。

#### 字符串切片和字符边界

当需要处理字符串中的特定部分时，正确使用字符串切片至关重要。例如，提取字符串的第一个字符：

```rust
fn first_char(s: &str) -> Option<char> {
    s.chars().next()
}
```

#### 字符串迭代

在某些情况下，你可能需要检查字符串中的每个字符。使用`.chars()`方法可以实现这一点：

```rust
fn count_vowels(s: &str) -> usize {
    s.chars().filter(|&c| "aeiou".contains(c)).count()
}
```

#### 字符串与其他类型之间的转换

对于需要将字符串转换为数字进行计算的场景，`parse`方法是一个常用的工具：

```rust
fn parse_number(s: &str) -> Result<i32, std::num::ParseIntError> {
    s.parse::<i32>()
}
```

反之，将数字转换为字符串时，可以使用`to_string`方法或`format!`宏：

```rust
fn number_to_string(n: i32) -> String {
    format!("Number: {}", n)
}
```

### 结合使用扩展知识点和解题方法

通过掌握这些扩展的知识点和解题方法，你可以更加灵活和高效地处理Rust中的字符串数据。例如，可以创建一个函数，它接受一个字符串，对其进行复杂的处理（如去除空白、替换子字符串、计算特定字符的出现次数），然后返回处理后的结果或相关的信息。这不仅提高了你解决实际问题的能力，也让你能够更深入地理解Rust中字符串的工作原理及其与内存安全和效率之间的关系。
