# Exercise 53

- Name: ```errors3```
- Path: ```exercises/error_handling/errors3.rs```
#### Hint: 

If other functions can return a `Result`, why shouldn't `main`? It's a fairly common convention to return something like Result<(), ErrorType> from your main function. The unit (`()`) type is there because nothing is really needed in terms of positive results.


---



```rust,editable
// errors3.rs
//
// This is a program that is trying to use a completed version of the
// `total_cost` function from the previous exercise. It's not working though!
// Why not? What should we do to fix it?
//
// Execute `rustlings hint errors3` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

use std::num::ParseIntError;

fn main() {
    let mut tokens = 100;
    let pretend_user_input = "8";

    let cost = total_cost(pretend_user_input)?;

    if cost > tokens {
        println!("You can't afford that many!");
    } else {
        tokens -= cost;
        println!("You now have {} tokens.", tokens);
    }
}

pub fn total_cost(item_quantity: &str) -> Result<i32, ParseIntError> {
    let processing_fee = 1;
    let cost_per_item = 5;
    let qty = item_quantity.parse::<i32>()?;

    Ok(qty * cost_per_item + processing_fee)
}

```

---

### 本题内容
本题目的在于理解并实现 Rust 中错误处理的基本概念。具体地，通过修改 `main` 函数使其能够处理 `Result` 类型，学习如何在 Rust 程序中正确处理可能出现的错误。

### 相关知识点
1. **`Result` 类型**: Rust 中用于错误处理的枚举类型，包含 `Ok(T)` 和 `Err(E)` 两种情况，其中 `T` 是操作成功时返回的类型，`E` 是错误类型。
2. **`?` 运算符**: 用于简化错误处理。当用在 `Result` 类型上时，如果值是 `Ok(T)`，则将 `T` 返回，如果是 `Err(E)`，则将 `E` 向上传递出函数。
3. **改造 `main` 函数**: Rust 允许 `main` 函数返回 `Result`，通常用来在程序顶级处理错误。

### 解题方法
- 修改 `main` 函数签名，使其返回 `Result<(), ParseIntError>`。
- 在 `main` 函数内部，适当使用 `?` 运算符处理可能发生的错误。
- 确保所有可能的错误都被妥善处理或传递至上层调用者。

### 代码示例
以下是修改后的代码示例：

```rust
use std::num::ParseIntError;

// 修改 main 函数，使其能返回 Result 类型
fn main() -> Result<(), ParseIntError> {
    let mut tokens = 100;
    let pretend_user_input = "8";

    // 使用 '?' 运算符来处理可能的错误
    let cost = total_cost(pretend_user_input)?;

    if cost > tokens {
        println!("You can't afford that many!");
    } else {
        tokens -= cost;
        println!("You now have {} tokens.", tokens);
    }

    // 当 main 函数正确执行完成时，返回 Ok(())
    Ok(())
}

pub fn total_cost(item_quantity: &str) -> Result<i32, ParseIntError> {
    let processing_fee = 1;
    let cost_per_item = 5;
    let qty = item_quantity.parse::<i32>()?;

    Ok(qty * cost_per_item + processing_fee)
}
```

这个示例中：
- `main` 函数通过返回 `Result<(), ParseIntError>`，允许在函数内使用 `?` 操作符。
- 通过 `item_quantity.parse::<i32>()?` 直接处理字符串解析的可能错误，并在错误发生时提早退出函数。
- 在结尾通过 `Ok(())` 明确表示成功执行完毕。

## 扩展知识点与解答：

### 扩展知识点

1. **错误传播**：在 Rust 中，使用 `?` 运算符可以简化错误处理流程。`?` 运算符在遇到 `Err` 时会停止当前函数的执行，并将错误传递给调用者。这是 Rust 鼓励使用的错误处理模式，以确保错误被妥善管理而不是被忽视。

2. **自定义错误类型**：在更复杂的应用中，使用标准库的错误类型（如 `ParseIntError`）可能不足以表达所有错误情况。Rust 允许你定义自己的错误类型，通常通过实现 `std::fmt::Debug` 和 `std::fmt::Display` trait 来完成。

3. **`Result` 和 `Option` 类型的转换**：在 Rust 中，`Option` 类型表示一个可能不存在的值，而 `Result` 类型用于可能会出错的操作。两者之间可以通过各种方法转换，例如使用 `ok_or` 或 `ok_or_else` 方法将 `Option` 转换为 `Result`。

4. **`unwrap` 和 `expect`**：这两个方法用于直接从 `Result` 或 `Option` 中提取值，但如果结果是 `Err` 或 `None`，它们会引起程序崩溃。`expect` 允许添加自定义错误消息，提供错误发生时的上下文。

### 扩展解题方法

#### 1. 理解 `Result` 和 `Option`
深入理解 `Result` 和 `Option` 的使用场景和转换方法，可以帮助在不同上下文中更好地管理程序中的潜在错误和缺失值。

#### 2. 定义和使用自定义错误
通过定义自己的错误类型来提高错误信息的清晰度和程序的健壮性。例如，可以创建一个枚举来描述不同的错误情况，再为其实现相关的 trait。

#### 3. 错误处理的最佳实践
- **避免过度使用 `unwrap` 和 `expect`**：虽然这些方法在原型开发或测试时很方便，但在生产代码中应当尽量避免，以免程序在未处理的错误情况下崩溃。
- **合理利用错误传递**：使用 `?` 运算符可以优雅地处理错误，确保错误信息不会丢失，并使得错误处理代码更加简洁。

### 应用到当前练习

在当前的 `errors3.rs` 练习中，通过实际操作实现了基本的错误处理，学习了如何修改 `main` 函数以返回 `Result` 类型，以及如何在函数中使用 `?` 来处理错误。这为深入学习 Rust 中的错误处理机制打下了坚实的基础。希望进一步扩展错误处理能力可以尝试定义自定义错误类型，并在项目中实践上述错误处理的最佳实践。
