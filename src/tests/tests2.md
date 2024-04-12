# Exercise 69

- Name: ```tests2```
- Path: ```exercises/tests/tests2.rs```
#### Hint: 

Like the previous exercise, you don't need to write any code to get this test to compile and run. `assert_eq!` is a macro that takes two arguments and compares them. Try giving it two values that are equal! Try giving it two arguments that are different! Try giving it two values that are of different types! Try switching which argument comes first and which comes second!


---



```rust,editable
// tests2.rs
//
// This test has a problem with it -- make the test compile! Make the test pass!
// Make the test fail!
//
// Execute `rustlings hint tests2` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

#[cfg(test)]
mod tests {
    #[test]
    fn you_can_assert_eq() {
        assert_eq!();
    }
}

```

---

### 本题内容
本练习的目的是帮助你深入理解和使用 Rust 中的 `assert_eq!` 宏。通过使用这个宏，你将学会如何验证代码中两个表达式的值是否相等，这是编写自动化测试的一个基本技能。

### 相关知识点
1. **`assert_eq!` 宏**：
   - `assert_eq!(a, b)`：检查两个表达式 `a` 和 `b` 是否相等。如果不等，它将导致 panic，并报告两个值。
   - 适用于确保程序的不同部分在逻辑上保持一致性，非常适合单元测试。

2. **测试的作用**：
   - 验证代码逻辑：确保代码按预期工作。
   - 防止回归：确保已修复的错误不会在未来的代码更新中重新出现。

3. **编译器检查**：
   - 类型检查：`assert_eq!` 要求两个参数类型相同，这由编译器在编译时进行强制。

### 解题方法
#### 步骤描述
1. **修复编译错误**：
   - 提供两个相同类型的参数给 `assert_eq!`。

2. **使测试通过**：
   - 为 `assert_eq!` 提供两个相等的值。

3. **使测试失败**：
   - 为 `assert_eq!` 提供两个不相等的值，并观察测试失败时的输出。

#### 代码示例 
修改后的测试代码如下所示：

```rust
#[cfg(test)]
mod tests {
    #[test]
    fn you_can_assert_eq() {
        // 使测试通过
        assert_eq!(3, 3, "This should pass because both values are the same.");

        // 若要使测试失败，可以取消注释以下行并注释上一行
        // assert_eq!(3, 4, "This should fail because the values are different.");
    }
}
```

### 扩展内容
- **扩展使用**：
  - **不同类型测试**：尝试使用不同类型的值，如 `assert_eq!("hello", "hello")` 或 `assert_eq!(vec![1, 2], vec![1, 2])`，来探索 `assert_eq!` 如何用于更复杂的数据结构。
  - **顺序无关性**：尝试交换 `assert_eq!` 的参数顺序，理解它如何检查两个参数的等价性而不关心顺序。

通过上述步骤和扩展内容的学习，你不仅能掌握如何使用 `assert_eq!` 进行基本的单元测试，还能理解其在复杂数据结构和不同类型值间的应用。这将大大增强你在实际项目中进行错误检查和防止回归的能力。

## 扩展知识点与解答：

### 扩展知识点

1. **类型一致性**:
   - `assert_eq!` 宏要求其两个参数具有相同的类型。这强制在比较时进行类型一致性检查，帮助发现潜在的类型错误。

2. **测试中的错误消息**:
   - `assert_eq!` 宏允许在其后附加一个自定义错误消息作为可选参数。这个消息在断言失败时会显示，提供了更多的上下文信息，有助于快速诊断问题。

3. **Panic 的捕获**:
   - 在测试中，`assert_eq!` 导致的 panic 被测试框架捕获，这并不会导致程序完全崩溃，而是标记为测试失败。这是 Rust 测试策略的一部分，旨在确保测试的健壮性和可靠性。

4. **使用 `assert_ne!` 进行否定测试**:
   - 与 `assert_eq!` 相对的是 `assert_ne!`，它用于验证两个表达式的值不相等。这在需要确认两个值或变量在逻辑上应该不相等的情况下非常有用。

### 扩展解题方法

1. **编写更多的测试用例**:
   - 对于给定的函数或模块，考虑所有可能的输入情况，编写多个测试用例来覆盖这些情况。这包括边界值、异常值和正常值。

2. **结合使用 `assert!` 和 `assert_eq!`**:
   - 在一些测试中，可能需要先进行一些布尔条件判断（使用 `assert!`），然后再进行值的比较（使用 `assert_eq!`）。这样可以更详细地控制测试逻辑和输出。

3. **参数化测试**:
   - 考虑使用参数化测试技术，例如在测试函数中循环多个不同的输入和预期输出。这样可以减少重复代码，并且使测试逻辑更清晰。

4. **探索集成测试**:
   - 对于更大的功能，编写集成测试而不仅仅是单元测试。集成测试涉及多个模块的交互，更能确保整体功能的正确性。

通过这些扩展知识点和解题方法，可以更深入地理解和应用 Rust 中的测试机制，不仅提高代码质量，还能确保软件的可维护性和扩展性。在实际项目中，这些测试技巧将是不可或缺的工具。

