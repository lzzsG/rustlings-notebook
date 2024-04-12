# Exercise 70

- Name: ```tests3```
- Path: ```exercises/tests/tests3.rs```
#### Hint: 

You can call a function right where you're passing arguments to `assert!` -- so you could do something like `assert!(having_fun())`. If you want to check that you indeed get false, you can negate the result of what you're doing using `!`, like `assert!(!having_fun())`.


---



```rust,editable
// tests3.rs
//
// This test isn't testing our function -- make it do that in such a way that
// the test passes. Then write a second test that tests whether we get the
// result we expect to get when we call `is_even(5)`.
//
// Execute `rustlings hint tests3` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

pub fn is_even(num: i32) -> bool {
    num % 2 == 0
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn is_true_when_even() {
        assert!();
    }

    #[test]
    fn is_false_when_odd() {
        assert!();
    }
}

```

---

### 本题内容
本练习的目标是通过编写单元测试来验证 `is_even` 函数的行为是否符合预期。这将帮助你熟悉如何使用 Rust 的测试框架进行基本的布尔逻辑测试，并确保函数在不同输入下的行为正确。

### 相关知识点
1. **`assert!` 宏**：
   - `assert!` 用于验证表达式的布尔值。如果表达式为 `true`，测试通过；如果为 `false`，测试失败并报错。
   
2. **函数测试**：
   - 测试是确保函数按预期工作的关键工具，尤其是在输入输出明确的函数，如 `is_even`。

3. **布尔函数测试**：
   - 对返回布尔值的函数进行测试通常包括至少两种情况：期望返回 `true` 和 `false` 的情况。

### 解题方法
#### 步骤描述
1. **编写测试以验证偶数情况**：
   - 测试 `is_even` 函数对偶数输入是否返回 `true`。
   
2. **编写测试以验证奇数情况**：
   - 测试 `is_even` 函数对奇数输入是否返回 `false`。

#### 代码示例
下面的代码展示了如何实现上述测试步骤：

```rust
pub fn is_even(num: i32) -> bool {
    num % 2 == 0
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn is_true_when_even() {
        assert!(is_even(2), "Expected true for even number, but got false");
    }

    #[test]
    fn is_false_when_odd() {
        assert!(!is_even(5), "Expected false for odd number, but got true");
    }
}
```

### 详细解释
- **`is_true_when_even` 测试**：
   - 这个测试验证 `is_even` 函数在传入偶数 `2` 时是否正确地返回 `true`。
   - 使用 `assert!` 宏直接调用 `is_even(2)` 并期望结果为 `true`。

- **`is_false_when_odd` 测试**：
   - 这个测试检查 `is_even` 函数在传入奇数 `5` 时是否正确地返回 `false`。
   - 使用 `assert!(!is_even(5))` 验证函数返回 `false`，`!` 运算符用于反转 `is_even` 的结果，确保测试框架期望的是 `true`（即函数的原始返回值应为 `false`）。

通过上述步骤和代码示例，你可以清楚地看到如何为简单的布尔返回函数编写有效的单元测试，确保函数在预期的输入上表现正确。这些基本的测试技巧是保证代码质量和后续功能扩展不引入错误的重要手段。

## 扩展知识点与解答：

### 扩展知识点

1. **参数化测试**:
   - 参数化测试允许开发者使用不同的输入重复执行同一测试逻辑。在 Rust 中，虽然标准库不直接支持参数化测试，但可以通过循环、数组或向量中的元素来模拟。这样做可以极大地增加测试的覆盖范围并减少代码重复。

2. **测试覆盖率**:
   - 测试覆盖率是衡量测试完整性的一个指标，表示代码中有多少百分比被测试覆盖。使用工具如 `tarpaulin` 或 `grcov` 等可以帮助在 Rust 项目中测量测试覆盖率，从而识别未被测试的代码区域。

3. **错误和异常处理的测试**:
   - 在函数可能抛出错误或异常的情况下，确保这些情况通过适当的测试进行处理是非常重要的。在 Rust 中，可以通过 `Result` 类型或 `panic!` 宏来管理错误和异常，测试这些场景确保软件的健壮性。

4. **利用 Mock 对象进行测试**:
   - 当测试的函数依赖于外部服务或复杂的对象时，使用 mock 对象可以是一种有效的策略。Mock 对象允许开发者模拟这些依赖项的行为，使得测试更加集中和高效。

### 扩展解题方法

1. **编写更全面的边界测试**:
   - 对于 `is_even` 函数，除了常规的奇偶数测试外，还应考虑边界值，如 `0`、极大或极小的整数，以及负数。这可以确保函数在所有预期的输入范围内正常工作。

2. **使用循环进行参数化测试**:
   - 创建一个包含多个测试案例的数组，例如 `[(2, true), (5, false), (0, true)]`，然后在测试函数中遍历这些案例，为每个元组中的值调用 `assert_eq!`。

   ```rust
   #[test]
   fn test_is_even_with_multiple_cases() {
       let test_cases = [(2, true), (5, false), (0, true)];
       for (input, expected) in test_cases.iter() {
           assert_eq!(is_even(*input), *expected, "Failed on input {}", input);
       }
   }
   ```

3. **测试异常情况的处理**:
   - 如果 `is_even` 函数在未来可能被修改为处理异常值（如字符串输入或特殊字符），编写测试以确保这些异常被适当处理，尽管当前版本可能不需要。

4. **集成自动化测试工具**:
   - 在 CI/CD 管道中集成自动化测试，确保每次提交或部署之前自动运行测试。这有助于即时发现问题并修复，保持代码质量。

通过应用这些扩展的知识点和解题方法，你可以更全面地理解测试在软件开发中的重要性，同时提高代码的稳健性和可靠性。
