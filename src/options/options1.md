# Exercise 48

- Name: ```options1```
- Path: ```exercises/options/options1.rs```
#### Hint: 

Options can have a Some value, with an inner value, or a None value, without an inner value.
There's multiple ways to get at the inner value, you can use unwrap, or pattern match. Unwrapping
is the easiest, but how do you do it safely so that it doesn't panic in your face later?


---



```rust,editable
// options1.rs
//
// Execute `rustlings hint options1` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

// This function returns how much icecream there is left in the fridge.
// If it's before 10PM, there's 5 pieces left. At 10PM, someone eats them
// all, so there'll be no more left :(
fn maybe_icecream(time_of_day: u16) -> Option<u16> {
    // We use the 24-hour system here, so 10PM is a value of 22 and 12AM is a
    // value of 0 The Option output should gracefully handle cases where
    // time_of_day > 23.
    // TODO: Complete the function body - remember to return an Option!
    ???
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn check_icecream() {
        assert_eq!(maybe_icecream(9), Some(5));
        assert_eq!(maybe_icecream(10), Some(5));
        assert_eq!(maybe_icecream(23), Some(0));
        assert_eq!(maybe_icecream(22), Some(0));
        assert_eq!(maybe_icecream(25), None);
    }

    #[test]
    fn raw_value() {
        // TODO: Fix this test. How do you get at the value contained in the
        // Option?
        let icecreams = maybe_icecream(12);
        assert_eq!(icecreams, 5);
    }
}

```

---

这个练习介绍了`Option`枚举，它是Rust中处理可能缺失值的标准方式。`Option`类型可以是`Some(T)`，代表有值，或者`None`，代表没有值。

### 本题内容：

目标是编写一个函数`maybe_icecream`，根据一天中的时间来返回冰淇淋的剩余数量。在晚上10点之前，冰淇淋的数量是5；到了10点，冰淇淋被吃完了，剩余数量为0。如果时间超出正常范围（0-23小时），则返回`None`。

### 相关知识点：

- **Option枚举**：`Option<T>`用于在Rust中表示一个可能存在也可能不存在的值。`Some(T)`包含了一个类型为`T`的值，而`None`表示没有值。
- **模式匹配**：`match`语句是Rust中处理枚举，包括`Option`的主要方式。通过模式匹配，你可以检查`Option`是`Some`还是`None`，并据此执行相应的代码。
- **`unwrap`和`expect`方法**：这些方法可以用来从`Some(T)`中取出`T`，但如果`Option`是`None`，它们会引发panic。它们应该谨慎使用，尤其是在`Option`可能为`None`的情况下。
- **安全地处理`Option`**：`unwrap_or`、`unwrap_or_else`和`map`等方法提供了更安全、更灵活的方式来处理`Option`值。

### 解题方法：

1. **实现函数逻辑**：
   - 使用`if`语句检查`time_of_day`，根据时间返回`Some(5)`、`Some(0)`或`None`。

2. **安全地解包`Option`**：
   - 在测试`raw_value`中，你需要从`Option`中安全地获取值。考虑使用`unwrap_or`方法，这样即便`Option`是`None`，也可以提供一个默认值。

### 代码示例：

```rust
fn maybe_icecream(time_of_day: u16) -> Option<u16> {
    if time_of_day > 23 {
        None
    } else if time_of_day >= 22 {
        Some(0)
    } else {
        Some(5)
    }
}

// 测试`raw_value`部分
let icecreams = maybe_icecream(12).unwrap_or(0);
assert_eq!(icecreams, 5);
```

通过完成这个练习，你将会对`Option`类型以及Rust中处理可能不存在的值的方式有更深入的了解。

## 扩展知识点与解答：

这个练习深入探讨了`Option<T>`枚举的使用，特别是在处理可能没有值的情况下如何安全地进行操作。`Option<T>`是Rust的核心特性之一，它强制你在编码时就考虑到空值的可能性，这有助于避免在运行时出现空引用错误。

### 扩展知识点：

- **`Option<T>`的使用场景**：在任何可能返回"没有值"的情况下使用，例如从集合中查找元素、执行可能失败的计算等。
- **解包`Option<T>`**：尽管`unwrap`和`expect`直接且易于使用，但它们在值为`None`时会造成程序panic。因此，推荐使用更安全的方法处理`Option<T>`，如`match`语句或`if let`。
- **`Option<T>`的方法**：
  - `map`：如果有值，则对其应用函数，否则返回`None`。
  - `and_then`：用于链接多个返回`Option`的操作，如果任一步骤返回`None`，整个链条立即返回`None`。
  - `unwrap_or`、`unwrap_or_else`：提供默认值或通过闭包延迟计算默认值。

### 扩展解题方法：

1. **使用`match`语句安全处理`Option<T>`**：这是处理`Option<T>`最灵活的方式，可以根据是`Some`还是`None`执行不同的代码路径。

```rust
fn process_option(option: Option<u16>) -> u16 {
    match option {
        Some(value) => value, // 如果有值，直接返回该值
        None => 0, // 如果没有值，返回默认值0
    }
}
```

2. **使用`if let`作为`match`的简化**：如果只关心`Option<T>`中的`Some`情况，可以使用`if let`结构使代码更简洁。

```rust
fn process_option(option: Option<u16>) -> u16 {
    if let Some(value) = option {
        value
    } else {
        0
    }
}
```

3. **利用`Option<T>`的方法链**：对于复杂的`Option<T>`操作，可以利用其提供的方法进行链式调用，使代码更加清晰和简洁。

```rust
fn transform_option(option: Option<String>) -> Option<usize> {
    option.map(|s| s.trim()) // 如果有值，先去除字符串两端的空白
          .filter(|s| !s.is_empty()) // 确保字符串非空
          .map(|s| s.len()) // 将字符串转换为其长度
}
```
