# Exercise 71

- Name: ```tests4```
- Path: ```exercises/tests/tests4.rs```
#### Hint: 

We expect method `Rectangle::new()` to panic for negative values.

To handle that you need to add a special attribute to the test function.You can refer to the docs:

[https://doc.rust-lang.org/stable/book/ch11-01](https://doc.rust-lang.org/stable/book/ch11-01-writing-tests.html#checking-for-panics-with-should_panic) ([Chinese version](https://rustwiki.org/zh-CN/book/ch11-01-writing-tests.html#%E4%BD%BF%E7%94%A8-should_panic-%E6%A3%80%E6%9F%A5-panic))


---

```rust,editable
// tests4.rs
//
// Make sure that we're testing for the correct conditions!
//
// Execute `rustlings hint tests4` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

struct Rectangle {
    width: i32,
    height: i32
}

impl Rectangle {
    // Only change the test functions themselves
    pub fn new(width: i32, height: i32) -> Self {
        if width <= 0 || height <= 0 {
            panic!("Rectangle width and height cannot be negative!")
        }
        Rectangle {width, height}
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn correct_width_and_height() {
        // This test should check if the rectangle is the size that we pass into its constructor
        let rect = Rectangle::new(10, 20);
        assert_eq!(???, 10); // check width
        assert_eq!(???, 20); // check height
    }

    #[test]
    fn negative_width() {
        // This test should check if program panics when we try to create rectangle with negative width
        let _rect = Rectangle::new(-10, 10);
    }

    #[test]
    fn negative_height() {
        // This test should check if program panics when we try to create rectangle with negative height
        let _rect = Rectangle::new(10, -10);
    }
}

```

---

### 本题内容
这个练习要求测试一个 `Rectangle` 结构体的构造函数，确保其正确处理正数和负数输入。特别地，我们需要验证构造函数在接收到负数参数时会触发 panic，这是一种预期的错误处理方式。

### 相关知识点
1. **`should_panic` 测试属性**:
   - `#[should_panic]` 是一个测试属性，用于指示一个测试函数预期会导致 panic。当你想验证某段代码在出错时能否正确地失败（如通过触发 panic），这个属性非常有用。

2. **测试 panic 的正确性**:
   - 使用 `should_panic` 属性可以验证函数在特定错误条件下是否按预期触发 panic。它是测试错误处理逻辑的有效方式。

3. **使用 `assert_eq!`**:
   - `assert_eq!(left, right)` 用于检查两个表达式的值是否相等，如果不相等，测试将失败并显示两个值。

### 解题方法
#### 步骤描述
1. **编写一个正常条件测试**:
   - 测试 `Rectangle::new` 在接收正数宽度和高度时能否正确创建对象，并验证其属性。

2. **编写测试以验证负宽度的 panic**:
   - 使用 `should_panic` 属性来确保当构造函数接收到负宽度时会触发 panic。

3. **编写测试以验证负高度的 panic**:
   - 类似于负宽度的测试，确保当构造函数接收到负高度时也会触发 panic。

#### 代码示例
这是上述步骤的具体实现：

```rust
struct Rectangle {
    width: i32,
    height: i32
}

impl Rectangle {
    pub fn new(width: i32, height: i32) -> Self {
        if width <= 0 || height <= 0 {
            panic!("Rectangle width and height cannot be negative!");
        }
        Rectangle { width, height }
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn correct_width_and_height() {
        let rect = Rectangle::new(10, 20);
        assert_eq!(rect.width, 10, "Width should be 10");
        assert_eq!(rect.height, 20, "Height should be 20");
    }

    #[test]
    #[should_panic(expected = "Rectangle width and height cannot be negative!")]
    fn negative_width() {
        let _rect = Rectangle::new(-10, 10);
    }

    #[test]
    #[should_panic(expected = "Rectangle width and height cannot be negative!")]
    fn negative_height() {
        let _rect = Rectangle::new(10, -10);
    }
}
```

### 详细解释
- **正常条件的测试** (`correct_width_and_height`):
  - 验证 `Rectangle::new` 在接收正数参数时能否正确创建对象，并检查其属性是否符合预期。

- **触发 panic 的测试** (`negative_width` 和 `negative_height`):
  - 使用 `should_panic` 属性来验证构造函数在接收到无效输入（负数宽度或高度）时是否如预期触发 panic。通过指定 `expected` 字符串，我们进一步确保 panic 消息符合预期，这有助于在实际应用中准确诊断问题。

这些测试不仅帮助确保 `Rectangle` 类的鲁棒性，也展示了如何在 Rust 中有效地管理和测试错误情况。

## 扩展知识点与解答：

### 扩展知识点

1. **Panic 捕获和处理**:
   - 在 Rust 中，`panic!` 是一个宏，用于在遇到无法处理的错误时终止程序执行。在测试中，使用 `should_panic` 属性可以帮助我们确认代码在预期的场景下正确地触发了 panic。了解如何捕获和响应 panic 对于编写可靠的错误处理逻辑至关重要。

2. **自定义错误消息**:
   - 在触发 panic 时，提供一个明确和具体的错误消息可以帮助更快地诊断问题。在实际应用中，合理使用自定义错误消息可以提高代码的可维护性和可读性。

3. **错误处理机制**:
   - 除了使用 panic 处理程序级别的错误外，Rust 还提供了 `Result` 类型，用于处理可以恢复的错误。区分何时使用 panic 和何时返回 `Result` 对于写出健壮的 Rust 程序非常重要。

4. **测试代码的隔离**:
   - 测试应当尽可能地隔离，确保不受外部状态的影响。这通常意味着在测试前设置必要的状态，并在测试后进行清理。在 Rust 中，这可以通过测试模块内的 setup 和 teardown 逻辑实现。

### 扩展解题方法

1. **增强测试的覆盖率**:
   - 考虑更多边缘情况和异常输入，确保 `Rectangle` 的构造函数在所有预期情况下都能正确地触发或避免 panic。例如，测试极限值如整数的最大值和最小值。

2. **使用条件编译**:
   - 对于仅在测试构建中运行的代码，可以使用 `#[cfg(test)]` 条件编译指令，确保这些代码不会包含在最终的生产二进制文件中。

3. **模拟测试**:
   - 对于涉及外部依赖的复杂系统，可以使用模拟对象 (mock objects) 来模拟外部依赖的行为。这有助于创建更可控和预测的测试环境。

4. **集成并发和异步代码的测试**:
   - 对于涉及并发或异步操作的代码，确保测试能够正确处理多线程或异步结果。这可能需要使用特定的测试策略，如使用 Futures 和 Tokio 提供的测试工具。

通过应用这些扩展知识点和解题方法，你可以更全面地理解和实践 Rust 中的测试策略，以及如何处理程序中的错误情况。这将有助于提高代码质量，减少生产环境中的故障率。
