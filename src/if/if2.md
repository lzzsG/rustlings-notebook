# Exercise 14

- Name: ```if2```
- Path: ```exercises/if/if2.rs```
#### Hint: 

For that first compiler error, it's important in Rust that each conditional block returns the same type! To get the tests passing, you will need a couple conditions checking different input values.


---



```rust,editable
// if2.rs
//
// Step 1: Make me compile!
// Step 2: Get the bar_for_fuzz and default_to_baz tests passing!
//
// Execute `rustlings hint if2` or use the `hint` watch subcommand for a hint.

// I AM NOT DONE

pub fn foo_if_fizz(fizzish: &str) -> &str {
    if fizzish == "fizz" {
        "foo"
    } else {
        1
    }
}

// No test changes needed!
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn foo_for_fizz() {
        assert_eq!(foo_if_fizz("fizz"), "foo")
    }

    #[test]
    fn bar_for_fuzz() {
        assert_eq!(foo_if_fizz("fuzz"), "bar")
    }

    #[test]
    fn default_to_baz() {
        assert_eq!(foo_if_fizz("literally anything"), "baz")
    }
}

```

---

### 本题内容：

这个练习关注于Rust中的类型一致性原则，特别是在使用`if`表达式时，每个分支必须返回相同类型的值。`foo_if_fizz`函数尝试在一个分支返回一个字符串，在另一个分支返回一个整数，这导致了编译错误。通过修改这个函数以返回相同类型的值，可以修复这个错误。

### 相关知识点：

- **类型一致性**：在Rust中，`if`表达式的所有分支必须返回相同类型的值。这是因为整个`if`表达式本身具有一个单一的返回类型。
- **字符串字面值的生命周期**：返回字符串字面值时，它们的类型是`&'static str`，意味着这些字符串字面值在整个程序运行期间都是有效的。

### 解题方法：

- **步骤描述**：
  - 修复编译错误需要确保`foo_if_fizz`函数的所有分支都返回同一类型的值。根据测试用例的期望，这个函数应该总是返回`&str`类型。
  - 根据测试用例的要求，我们需要根据`fizzish`的值返回不同的字符串。

- **代码示例**：
  

```rust
// if2.rs
// 已完成练习

pub fn foo_if_fizz(fizzish: &str) -> &str {
    if fizzish == "fizz" {
        "foo"
    } else if fizzish == "fuzz" {
        "bar"
    } else {
        "baz"
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn foo_for_fizz() {
        assert_eq!(foo_if_fizz("fizz"), "foo");
    }

    #[test]
    fn bar_for_fuzz() {
        assert_eq!(foo_if_fizz("fuzz"), "bar");
    }

    #[test]
    fn default_to_baz() {
        assert_eq!(foo_if_fizz("literally anything"), "baz");
    }
}
```
在这个修正后的版本中，`foo_if_fizz`函数现在检查`fizzish`的值，并根据该值返回相应的字符串。所有分支都返回`&str`类型，满足了Rust的类型一致性要求。通过这样的修改，函数能够成功编译，且通过所有提供的测试用例。

## 扩展知识点与解答：

深入`if2`练习之后，我们可以进一步探索在Rust中处理条件逻辑和返回值时的高级技巧，以及如何在单元测试中验证这些逻辑。

### 扩展知识点：

1. **匹配多个条件**：
   - Rust提供了强大的模式匹配功能，可以用来替代一系列`if-else if`语句。`match`表达式让代码更加清晰、易于管理。

2. **返回引用**：
   - 当函数返回字符串字面值时，这些字面值实际上是具有`'static`生命周期的引用。理解Rust的生命周期注解对于正确地返回引用类型的值非常重要。

3. **测试驱动开发（TDD）**：
   - 通过编写测试来指导功能实现是一种有效的开发方法。Rust的单元测试功能让这一方法易于应用。

### 扩展解题方法：

- **使用`match`表达式**：
  - 考虑使用`match`表达式重写`foo_if_fizz`函数，以提高其可读性和可维护性。

- **单元测试详解**：
  - Rust的单元测试框架允许你为代码编写测试，以验证其功能符合预期。通过`#[cfg(test)]`属性标记的模块仅在运行测试时编译。`assert_eq!`宏用于断言两个表达式的值是否相等，是验证函数返回值的常用方法。

- **代码示例（使用`match`表达式）**：
  

```rust
pub fn foo_if_fizz(fizzish: &str) -> &str {
    match fizzish {
        "fizz" => "foo",
        "fuzz" => "bar",
        _ => "baz",
    }
}
```

### 测试部分解释：

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn foo_for_fizz() {
        // 验证当参数为"fizz"时，函数返回"foo"
        assert_eq!("foo", foo_if_fizz("fizz"));
    }

    #[test]
    fn bar_for_fuzz() {
        // 验证当参数为"fuzz"时，函数返回"bar"
        assert_eq!("bar", foo_if_fizz("fuzz"));
    }

    #[test]
    fn default_to_baz() {
        // 验证对于其他任何输入，函数都返回"baz"
        assert_eq!("baz", foo_if_fizz("literally anything"));
    }
}
```
这些测试覆盖了`foo_if_fizz`函数的所有可能的输入情况，确保每种情况下都能得到正确的返回值。这种测试方法有助于确保函数逻辑的正确性和鲁棒性。
