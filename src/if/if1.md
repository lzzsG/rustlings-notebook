# Exercise 13

- Name: ```if1```
- Path: ```exercises/if/if1.rs```
#### Hint: 

It's possible to do this in one line if you would like! Some similar examples from other languages:
- In C(++) this would be: `a > b ? a : b`
- In Python this would be:  `a if a > b else b`
Remember in Rust that:
- the `if` condition does not need to be surrounded by parentheses
- `if`/`else` conditionals are expressions
- Each condition is followed by a `{}` block.


---



```rust,editable
// if1.rs
//
// Execute `rustlings hint if1` or use the `hint` watch subcommand for a hint.

// I AM NOT DONE

pub fn bigger(a: i32, b: i32) -> i32 {
    // Complete this function to return the bigger number!
    // Do not use:
    // - another function call
    // - additional variables
}

// Don't mind this for now :)
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn ten_is_bigger_than_eight() {
        assert_eq!(10, bigger(10, 8));
    }

    #[test]
    fn fortytwo_is_bigger_than_thirtytwo() {
        assert_eq!(42, bigger(32, 42));
    }
}

```

---

### 本题内容：

这个练习旨在通过实现一个简单的函数来练习Rust中的`if`表达式。函数`bigger`应该比较两个整数`a`和`b`，返回其中较大的一个。这个任务可以通过使用Rust的`if`表达式轻松完成，这与其他编程语言中的三元操作符类似。

### 相关知识点：

- **`if`表达式**：在Rust中，`if`可以用作表达式，意味着它可以根据条件计算并返回值。不需要用括号将条件括起来，但每个条件后必须跟一个代码块`{}`。
- **表达式 vs 语句**：Rust中的`if`表达式与其他语言中的三元操作符（`? :`）类似，允许在单行内根据条件返回不同的值。

### 解题方法：

- **步骤描述**：
  - 使用`if`表达式来比较`a`和`b`，根据比较结果返回较大的值。考虑到`if`/`else`在Rust中是表达式，我们可以直接在函数中返回它的结果。

- **代码示例**：
    

```rust
pub fn bigger(a: i32, b: i32) -> i32 {
    if a > b { a } else { b }
}
```
这个解决方案利用了`if`表达式的能力，直接比较`a`和`b`，并返回较大的那个值。由于`if`表达式本身就能返回值，所以这段代码非常简洁且高效。

通过完成这个练习，你将学会如何在Rust中使用`if`表达式来根据条件计算和返回值，这是编写条件逻辑的一种非常有用的技巧。

## 扩展知识点与解答：

深入探讨`if1`练习后，我们可以扩展到Rust中`if`表达式的更多用途，包括与模式匹配结合使用，以及如何在单元测试中验证函数行为。

### 扩展知识点：

1. **模式匹配与`if`表达式**：
   - Rust中的`match`表达式提供了一种强大的模式匹配能力，可以与`if`表达式结合使用来处理更复杂的条件逻辑。`if let`是`match`的一个便捷替代，用于处理只关心一种匹配模式的情况。

2. **单元测试**：
   - Rust的单元测试是一种验证代码功能和行为的方法。测试函数通常位于与它们测试的代码相同的文件中，并使用`#[cfg(test)]`属性来标注测试模块，确保测试代码只在执行测试时编译。

### 扩展解题方法：

- **探索复杂条件逻辑**：
  
  - 实践在函数中使用`if`和`match`表达式来处理更复杂的条件分支。这有助于编写能够处理多种场景的灵活代码。
  
- **编写和运行单元测试**：
  - 为你的函数编写单元测试，以验证其在不同输入下的行为。这是一个良好的编程实践，有助于确保代码的正确性和稳定性。

- **代码示例（使用`if let`进行模式匹配）**：
    

```rust
// 假设有一个返回Option<i32>的函数
fn get_optional_value(flag: bool) -> Option<i32> {
    if flag { Some(10) } else { None }
}

// 使用`if let`处理Option值
fn check_optional_value(flag: bool) {
    if let Some(value) = get_optional_value(flag) {
        println!("Got a value: {}", value);
    } else {
        println!("Got no value.");
    }
}
```

### 测试代码详解：

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn ten_is_bigger_than_eight() {
        assert_eq!(10, bigger(10, 8));
    }

    #[test]
    fn fortytwo_is_bigger_than_thirtytwo() {
        assert_eq!(42, bigger(32, 42));
    }
}
```
- `#[cfg(test)]`标注了一个专门用于测试的模块。这意味着，这部分代码在常规编译过程中会被忽略，只有在运行测试时才会被编译和执行。
- `use super::*;`导入了上一级作用域（本例中是外部函数`bigger`）的所有内容，以便在测试中使用。
- 每个`#[test]`函数是一个单独的测试案例，使用`assert_eq!`宏来检查`bigger`函数的返回值是否符合预期。
