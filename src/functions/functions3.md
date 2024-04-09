# Exercise 10

- Name: ```functions3```
- Path: ```exercises/functions/functions3.rs```
#### Hint: 

This time, the function *declaration* is okay, but there's something wrong with the place where we're calling the function.

As a reminder, you can freely play around with different solutions in Rustlings! Watch mode will only jump to the next exercise if you remove the I AM NOT DONE comment.


---



```rust,editable
// functions3.rs
//
// Execute `rustlings hint functions3` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

fn main() {
    call_me();
}

fn call_me(num: u32) {
    for i in 0..num {
        println!("Ring! Call number {}", i + 1);
    }
}

```

---

### 本题内容：

这个练习目的是让学生识别和修复函数调用时的错误。特别是，`call_me`函数期望一个`u32`类型的参数，但在调用时并没有提供这个参数。通过解决这个问题，学生将学习如何正确地调用带参数的函数。

### 相关知识点：

- **函数调用与参数传递**：在Rust中，调用函数时需要提供与函数定义匹配的参数。如果函数定义中指定了参数，那么在调用该函数时必须传入相应的参数值。
- **参数类型匹配**：传递给函数的参数类型必须与函数定义中声明的类型匹配。类型不匹配将导致编译错误。

### 解题方法：

- **步骤描述**：
  - 为了修复编译错误并正确调用`call_me`函数，我们需要在调用`call_me`时提供一个`u32`类型的参数。可以选择任意的`u32`值作为参数传递给`call_me`函数。

- **代码示例**：
    ```rust
    // functions3.rs
    // 已完成练习
    
    fn main() {
        call_me(3); // 向`call_me`函数传递了一个u32类型的参数
    }
    
    fn call_me(num: u32) {
        for i in 0..num {
            println!("Ring! Call number {}", i + 1);
        }
    }
    ```
    在这个修正后的版本中，我们通过向`call_me`函数传递一个参数`3`（一个`u32`类型的值）来满足函数定义的要求。这样，当`main`函数运行并调用`call_me(3)`时，它将按照期望打印三次"Ring! Call number X"，其中X是从1开始的序号。这个练习展示了如何在Rust中正确地调用需要参数的函数。

## 扩展知识点与解答：

深入探讨`functions3`练习后，我们可以扩展到更复杂的函数调用场景和参数传递技巧，包括默认参数、变长参数，以及参数传递的高级模式。

### 扩展知识点：

1. **默认参数**：
   - Rust不直接支持函数定义时指定默认参数值。但是，可以通过结构体和方法或者使用多个函数定义来模拟这个功能。

2. **变长参数（Variadic Functions）**：
   - Rust本身不支持直接定义接受任意数量参数的函数（如C语言中的variadic函数）。但是，可以使用切片或向量作为参数来处理变长的输入。

3. **参数传递模式**：
   - Rust中的参数可以按值传递、按引用传递（可变与不可变），或者使用更高级的模式，如模式匹配和解构，来在函数调用中传递和使用复杂类型的数据。

### 扩展解题方法：

- **模拟默认参数**：
  - 通过定义一个接受结构体实例为参数的函数，并为该结构体实现`Default`特征，可以模拟带有默认参数的函数。

- **处理变长参数**：
  - 定义一个接受向量或切片作为参数的函数，可以处理任意数量的输入。

- **代码示例（模拟默认参数）**：
    ```rust
    #[derive(Default)]
    struct CallParams {
        num: u32,
        // 可以添加更多字段
    }
    
    fn call_me_with_params(params: CallParams) {
        for i in 0..params.num {
            println!("Ring! Call number {}", i + 1);
        }
    }
    
    fn main() {
        let params = CallParams { num: 3, ..CallParams::default() };
        call_me_with_params(params);
    }
    ```
    这个示例中，`CallParams`结构体包含`call_me`函数所需的参数。通过实现`Default`特征，我们可以创建带有默认值的`params`实例，这类似于使用默认参数。
