# Exercise 54

- Name: ```errors4```
- Path: ```exercises/error_handling/errors4.rs```
#### Hint: 

`PositiveNonzeroInteger::new` is always creating a new instance and returning an `Ok` result. It should be doing some checking, returning an `Err` result if those checks fail, and only returning an `Ok` result if those checks determine that everything is... okay :)


---



```rust,editable
// errors4.rs
//
// Execute `rustlings hint errors4` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

#[derive(PartialEq, Debug)]
struct PositiveNonzeroInteger(u64);

#[derive(PartialEq, Debug)]
enum CreationError {
    Negative,
    Zero,
}

impl PositiveNonzeroInteger {
    fn new(value: i64) -> Result<PositiveNonzeroInteger, CreationError> {
        // Hmm...? Why is this only returning an Ok value?
        Ok(PositiveNonzeroInteger(value as u64))
    }
}

#[test]
fn test_creation() {
    assert!(PositiveNonzeroInteger::new(10).is_ok());
    assert_eq!(
        Err(CreationError::Negative),
        PositiveNonzeroInteger::new(-10)
    );
    assert_eq!(Err(CreationError::Zero), PositiveNonzeroInteger::new(0));
}

```

---

### 本题内容
这个练习要求你修改 `PositiveNonzeroInteger::new` 函数，以确保它在构造对象之前对输入值进行适当的检查。目标是确保只有正非零整数才能成功创建 `PositiveNonzeroInteger` 实例。如果输入值为零或负数，则应返回相应的错误。

### 相关知识点
1. **错误处理**：通过使用 `Result` 类型处理潜在的错误情况。
2. **自定义错误类型**：使用枚举定义特定的错误类型，提高错误处理的可读性和维护性。
3. **数据验证**：在构造对象之前验证数据，确保对象状态的有效性和程序的健壮性。

### 解题方法
- **修改 `PositiveNonzeroInteger::new` 函数**：在创建 `PositiveNonzeroInteger` 实例之前，先检查传入的 `i64` 值是否大于零。
- **返回错误**：如果检查失败（即如果值为零或负），则返回相应的 `CreationError`。

### 代码示例
这是根据题目要求修改后的 `PositiveNonzeroInteger::new` 函数的实现示例：

```rust
impl PositiveNonzeroInteger {
    fn new(value: i64) -> Result<PositiveNonzeroInteger, CreationError> {
        if value <= 0 {
            if value == 0 {
                return Err(CreationError::Zero);
            } else {
                return Err(CreationError::Negative);
            }
        }
        Ok(PositiveNonzeroInteger(value as u64))
    }
}

#[test]
fn test_creation() {
    assert!(PositiveNonzeroInteger::new(10).is_ok());
    assert_eq!(
        Err(CreationError::Negative),
        PositiveNonzeroInteger::new(-10)
    );
    assert_eq!(Err(CreationError::Zero), PositiveNonzeroInteger::new(0));
}
```

这个修改确保了：
- 当 `value` 为负时，返回 `CreationError::Negative`。
- 当 `value` 为零时，返回 `CreationError::Zero`。
- 只有当 `value` 是一个正整数时，函数才返回 `Ok(PositiveNonzeroInteger(value as u64))`，确保了实例的正整性。

通过这样的实践，你不仅学习了如何在 Rust 中进行有效的错误处理，还确保了数据的正确性和程序的稳定性。

## 扩展知识点与解答：

### 扩展知识点

1. **更复杂的错误处理**：在 Rust 中，错误处理不仅仅是返回 `Ok` 或 `Err`。通过使用更丰富的错误处理模式，如多态错误处理（使用 trait 对象），可以创建更灵活且强大的错误处理逻辑。

2. **错误链**：当错误从一个上下文传递到另一个上下文时，保留原始错误的信息非常重要，这就是所谓的错误链。Rust 的 `std::error::Error` trait 支持这一机制，使得可以通过 `source` 方法追踪到原始错误。

3. **自定义错误和 `derive` 宏**：Rust 允许自定义错误类型，并且可以利用 `derive` 属性自动为自定义错误类型实现 `Debug`、`Clone`、`Copy` 等 trait，简化开发过程。

4. **错误处理和程序设计**：良好的错误处理是程序设计的重要部分。它不仅关系到程序的稳定性和安全性，也关系到用户体验。设计清晰的错误处理逻辑，能够使得错误更易于理解和修复。

### 扩展解题方法

1. **深入理解 `Result` 和 `Option`**：`Result` 和 `Option` 是 Rust 错误处理的基石。深入理解它们的使用场景和方法，能够在不同情况下选择最合适的错误处理策略。

2. **利用 Rust 的高级错误处理特性**：探索如 `anyhow` 和 `thiserror` 这样的库，它们提供了更高级的错误处理能力，如自定义错误的定义和自动从底层错误类型转换。

3. **错误处理的最佳实践**：开发过程中，应遵循一些最佳实践，如避免使用 `unwrap` 和 `expect`，使用 `?` 传播错误，以及创建有意义的自定义错误类型，提高错误的可读性和可维护性。

4. **编写单元测试来测试错误情况**：通过单元测试来测试代码中的错误路径，确保错误被正确处理，同时也是验证错误处理逻辑正确性的好方法。

### 应用到当前练习

在当前的 `errors4` 练习中，扩展学习内容可以应用于对 `CreationError` 类型的进一步开发，比如实现 `std::fmt::Display` 和 `std::error::Error` 以提供更友好的错误信息。此外，可以通过添加更多的单元测试来覆盖各种错误情况，确保错误处理逻辑的健壮性。通过这些扩展方法，不仅能够解决当前的练习，还能提升对 Rust 错误处理更深层次的理解。
