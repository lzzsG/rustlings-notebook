# Exercise 52

- Name: ```errors2```
- Path: ```exercises/error_handling/errors2.rs```
#### Hint: 

One way to handle this is using a `match` statement on `item_quantity.parse::<i32>()` where the cases are `Ok(something)` and `Err(something)`. 

This pattern is very common in Rust, though, so there's a `?` operator that does pretty much what you would make that match statement do for you! Take a look at this section of the Error Handling chapter:

[https://doc.rust-lang.org/book/ch09-02](https://doc.rust-lang.org/book/ch09-02-recoverable-errors-with-result.html#a-shortcut-for-propagating-errors-the--operatorand)  ([Chinese version](https://rustwiki.org/zh-CN/book/ch09-02-recoverable-errors-with-result.html)) give it a try!


---



```rust,editable
// errors2.rs
//
// Say we're writing a game where you can buy items with tokens. All items cost
// 5 tokens, and whenever you purchase items there is a processing fee of 1
// token. A player of the game will type in how many items they want to buy, and
// the `total_cost` function will calculate the total cost of the tokens. Since
// the player typed in the quantity, though, we get it as a string-- and they
// might have typed anything, not just numbers!
//
// Right now, this function isn't handling the error case at all (and isn't
// handling the success case properly either). What we want to do is: if we call
// the `parse` function on a string that is not a number, that function will
// return a `ParseIntError`, and in that case, we want to immediately return
// that error from our function and not try to multiply and add.
//
// There are at least two ways to implement this that are both correct-- but one
// is a lot shorter!
//
// Execute `rustlings hint errors2` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

use std::num::ParseIntError;

pub fn total_cost(item_quantity: &str) -> Result<i32, ParseIntError> {
    let processing_fee = 1;
    let cost_per_item = 5;
    let qty = item_quantity.parse::<i32>();

    Ok(qty * cost_per_item + processing_fee)
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn item_quantity_is_a_valid_number() {
        assert_eq!(total_cost("34"), Ok(171));
    }

    #[test]
    fn item_quantity_is_an_invalid_number() {
        assert_eq!(
            total_cost("beep boop").unwrap_err().to_string(),
            "invalid digit found in string"
        );
    }
}

```

### 本题内容

本练习的目标是改进一个计算游戏物品购买总成本的函数。玩家输入想要购买的物品数量（作为字符串），函数计算总成本。因为输入是字符串，所以存在解析错误的可能，我们需要合理地处理这种错误情况。

### 相关知识点

- **`Result` 类型**：用于可能会出错的操作。`Result<T, E>` 有两个变体：`Ok(T)` 表示操作成功，`Err(E)` 表示出错。
- **字符串解析**：`parse` 方法尝试将字符串转换为某种类型的值，可能会失败并返回 `ParseIntError` 错误。
- **`?` 操作符**：简化错误处理。如果 `Result` 是 `Ok`，则结果被解包继续执行；如果是 `Err`，则立即从当前函数返回 `Err`。

### 解题方法

#### 使用 `?` 操作符

`?` 操作符可以大幅简化错误处理。在这个例子中，我们可以直接在 `parse` 方法调用后使用 `?`，如果 `parse` 成功，就继续执行；如果失败，则立即返回 `Err(ParseIntError)`。

```rust
pub fn total_cost(item_quantity: &str) -> Result<i32, ParseIntError> {
    let processing_fee = 1;
    let cost_per_item = 5;
    let qty = item_quantity.parse::<i32>()?;

    Ok(qty * cost_per_item + processing_fee)
}
```

#### 使用 `match` 语句

另一种方法是使用 `match` 语句显式处理 `Result`。这种方法更加冗长，但在某些情况下可以提供更大的灵活性。

```rust
pub fn total_cost(item_quantity: &str) -> Result<i32, ParseIntError> {
    let processing_fee = 1;
    let cost_per_item = 5;
    let qty_result = item_quantity.parse::<i32>();

    match qty_result {
        Ok(qty) => Ok(qty * cost_per_item + processing_fee),
        Err(e) => Err(e),
    }
}
```

### 代码示例

使用 `?` 操作符是处理这类问题的推荐方式，因为它更简洁，并且能够有效地传播错误。以上示例展示了如何使用 `?` 操作符简化错误处理，并且通过直接返回 `parse` 方法可能产生的错误，使函数更加健壮。

理解如何使用 `Result` 类型和 `?` 操作符对于有效地进行错误处理是非常重要的，这是 Rust 语言中管理和传播错误的核心机制之一。

## 扩展知识点与解答：

### 扩展知识点

在 Rust 中，错误处理是一个核心概念，主要通过 `Result` 和 `Option` 枚举来实现。`Result` 类型尤其用于那些可能会失败的操作，其中 `Ok` 分支表示成功并包含结果值，而 `Err` 分支表示错误并包含错误信息。这个练习主要关注 `Result` 类型的使用，以及如何优雅地处理可能出现的错误。

### 扩展解答方法

#### 更深入地使用 `?` 操作符

`?` 操作符不仅可以用在 `Result` 类型上，还可以用于 `Option` 类型，条件是必须将 `Option` 转换为 `Result`。在处理错误时，`?` 操作符能够简化代码，使其更加清晰和直观。此外，当需要在错误发生时执行某些清理操作或额外逻辑时，可以考虑结合使用 `?` 操作符和闭包或高阶函数。

#### 自定义错误类型

在复杂的应用中，可能需要定义自定义错误类型来更准确地描述可能发生的错误情况。通过实现 `std::error::Error` 特质（trait）和使用枚举来表示不同的错误类型，可以创建更灵活和表达力更强的错误处理机制。

例如：

```rust
use std::num::ParseIntError;
use std::fmt;

#[derive(Debug)]
enum PurchaseError {
    ParseError(ParseIntError),
    // 其他错误类型
}

impl fmt::Display for PurchaseError {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        match *self {
            PurchaseError::ParseError(ref e) => e.fmt(f),
            // 处理其他错误类型
        }
    }
}

impl From<ParseIntError> for PurchaseError {
    fn from(err: ParseIntError) -> PurchaseError {
        PurchaseError::ParseError(err)
    }
}

// 使用 PurchaseError 作为错误类型
pub fn total_cost(item_quantity: &str) -> Result<i32, PurchaseError> {
    let processing_fee = 1;
    let cost_per_item = 5;
    let qty = item_quantity.parse::<i32>()?;

    Ok(qty * cost_per_item + processing_fee)
}
```

这种扩展方法使得错误处理不仅限于直接处理标准库中的错误类型，还可以根据应用的需要，定义和组织自定义错误类型。这对于大型项目和库的开发尤为重要，因为它可以提供更丰富的错误信息，并允许调用者更灵活地处理各种错误情况。

总之，理解和掌握 Rust 的错误处理机制是编写健壮、可维护代码的关键。通过有效地使用 `Result` 类型、`?` 操作符以及自定义错误类型，可以在保持代码清晰性的同时，优雅地处理错误情况。
