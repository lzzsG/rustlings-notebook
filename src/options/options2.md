# Exercise 49

- Name: ```options2```
- Path: ```exercises/options/options2.rs```
#### Hint: 

check out:

[https://doc.rust-lang.org/rust-by-example/flow_control/if_let.html](https://doc.rust-lang.org/rust-by-example/flow_control/if_let.html) ([Chinese version](https://rustwiki.org/zh-CN/rust-by-example/flow_control/if_let.html))

[https://doc.rust-lang.org/rust-by-example/flow_control/while_let.html](https://doc.rust-lang.org/rust-by-example/flow_control/while_let.html) ([Chinese version]())

Remember that Options can be stacked in if let and while let.

For example: `Some(Some(variable)) = variable2`
Also see `Option::flatten`



---



```rust,editable
// options2.rs
//
// Execute `rustlings hint options2` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

#[cfg(test)]
mod tests {
    #[test]
    fn simple_option() {
        let target = "rustlings";
        let optional_target = Some(target);

        // TODO: Make this an if let statement whose value is "Some" type
        word = optional_target {
            assert_eq!(word, target);
        }
    }

    #[test]
    fn layered_option() {
        let range = 10;
        let mut optional_integers: Vec<Option<i8>> = vec![None];

        for i in 1..(range + 1) {
            optional_integers.push(Some(i));
        }

        let mut cursor = range;

        // TODO: make this a while let statement - remember that vector.pop also
        // adds another layer of Option<T>. You can stack `Option<T>`s into
        // while let and if let.
        integer = optional_integers.pop() {
            assert_eq!(integer, cursor);
            cursor -= 1;
        }

        assert_eq!(cursor, 0);
    }
}

```

---

这个练习引导你深入理解 Rust 中的 `Option<T>` 类型，特别是如何使用 `if let` 和 `while let` 来处理嵌套的 `Option<T>`。这些构造使得解包 `Option<T>` 变得更加灵活且安全，避免了使用 `unwrap()` 可能导致的 panic。

### 本题内容：
- 学习如何通过 `if let` 和 `while let` 语句来优雅地处理 `Option<T>` 类型。
- 理解嵌套 `Option<T>` 类型的解包方法。

### 相关知识点：
- **`if let`**：允许你将 `Option<T>` 解包为 `Some(T)` 并执行某个代码块，而忽略 `None` 情况。
- **`while let`**：对 `if let` 的循环版本，允许你在 `Option<T>` 为 `Some(T)` 时循环执行某个代码块。
- **嵌套 `Option<T>`**：当 `Option<T>` 本身包含另一个 `Option<T>` 时，可以通过多层 `if let` 或 `while let` 来解包。

### 解题方法：

1. **简单选项处理**：使用 `if let` 语句来解包 `optional_target` 并验证它是否等于 `target`。
```rust
if let Some(word) = optional_target {
    assert_eq!(word, target);
}
```

2. **处理嵌套选项**：通过 `while let` 语句循环解包 `optional_integers.pop()`，这本身就是一个 `Option<Option<i8>>`，直到 `optional_integers` 为空。
```rust
while let Some(Some(integer)) = optional_integers.pop() {
    assert_eq!(integer, cursor);
    cursor -= 1;
}
```

### 代码示例：

```rust
#[cfg(test)]
mod tests {
    #[test]
    fn simple_option() {
        let target = "rustlings";
        let optional_target = Some(target);

        // 使用 if let 处理简单的 Option<T>
        if let Some(word) = optional_target {
            assert_eq!(word, target);
        }
    }

    #[test]
    fn layered_option() {
        let range = 10;
        let mut optional_integers: Vec<Option<i8>> = vec![None];

        for i in 1..(range + 1) {
            optional_integers.push(Some(i));
        }

        let mut cursor = range;

        // 使用 while let 处理嵌套的 Option<T>
        while let Some(Some(integer)) = optional_integers.pop() {
            assert_eq!(integer, cursor);
            cursor -= 1;
        }

        assert_eq!(cursor, 0);
    }
}
```

通过这个练习，你应该对如何使用 `if let` 和 `while let` 来安全且有效地处理 `Option<T>` 有了更深的理解。这些是 Rust 中处理可能缺失值的强大工具，有助于编写更健壮的代码。

## 扩展知识点与解答：

这个练习进一步深入了解 `Option<T>` 类型，尤其是如何处理嵌套的 `Option<Option<T>>` 结构，并引入了 `Option::flatten` 方法来简化嵌套 `Option` 的处理。

### 扩展知识点：

- **`Option::flatten`**：当你有一个 `Option<Option<T>>` 时，`flatten` 方法可以将其转换为单一层的 `Option<T>`。这对于简化对嵌套 `Option` 结构的处理非常有用。

### 扩展解题方法：

除了使用 `if let` 和 `while let` 来处理嵌套的 `Option`，`Option::flatten` 提供了一个更简洁的方式来简化这一过程。

1. **使用 `flatten` 处理嵌套的 `Option`**：在 `while let` 循环中，你可能遇到 `Option<Option<T>>` 类型。使用 `flatten` 方法，你可以将它转换为 `Option<T>`，使得代码更加直接和简洁。

### 代码示例：

修改 `layered_option` 测试中的 `while let` 循环，利用 `flatten` 来简化嵌套的 `Option` 处理：

```rust
#[test]
fn layered_option() {
    let range = 10;
    let mut optional_integers: Vec<Option<i8>> = vec![None];

    for i in 1..(range + 1) {
        optional_integers.push(Some(i));
    }

    let mut cursor = range;

    // 使用 flatten 简化嵌套 Option 的处理
    while let Some(integer) = optional_integers.pop().flatten() {
        assert_eq!(integer, cursor);
        cursor -= 1;
    }

    assert_eq!(cursor, 0);
}
```

通过引入 `Option::flatten`，你可以更简洁地处理复杂的嵌套 `Option` 结构，减少代码的复杂性。这在处理多层可选值时尤其有用，可以让你的代码更加清晰和易于维护。
