# Exercise 8

- Name: ```functions1```
- Path: ```exercises/functions/functions1.rs```
#### Hint: 

This main function is calling a function that it expects to exist, but the function doesn't exist. It expects this function to have the name `call_me`.

It expects this function to not take any arguments and not return a value. Sounds a lot like `main`, doesn't it?


---



```rust,editable
// functions1.rs
//
// Execute `rustlings hint functions1` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

fn main() {
    call_me();
}

```

---

### 本题内容：

这个练习的目的是让学生了解如何在Rust中定义和调用函数。特别是，它指出了一个程序可能会尝试调用一个不存在的函数，导致编译错误。通过创建一个符合期望的`call_me`函数，学生将学习函数定义的基本语法，包括函数名、参数列表（在本例中为空），以及如何在没有返回值的情况下声明函数。

### 相关知识点：

- **函数定义**：在Rust中，函数通过`fn`关键字定义，后面跟着函数名、参数列表和函数体。如果函数不返回值，则不需要指定返回类型。
- **无参数函数**：如果函数不接受任何参数，那么其参数列表为空，即`()`。
- **无返回值**：在Rust中，如果函数不返回任何值，实际上它返回一个空元组`()`，这在Rust中被称为单元类型（unit type）。不需要显式地声明这种返回类型。

### 解题方法：

- **步骤描述**：
  - 为了解决编译错误，我们需要定义一个名为`call_me`的函数。这个函数不应接受任何参数，并且不返回任何值。
  - 在`call_me`函数体内，你可以添加任何操作，比如打印一条消息，或者什么都不做。

- **代码示例**：
    

```rust
// functions1.rs
// 已完成练习

fn main() {
    call_me();
}

// 定义一个不接受参数也不返回值的函数`call_me`
fn call_me() {
    println!("Called `call_me()`");
}
```
在这个修正后的版本中，我们添加了一个新的函数`call_me`，正如`main`函数所期望的那样。当`main`函数运行并调用`call_me`时，它将执行`call_me`函数体内的代码，本例中是打印一条消息。这样，程序就能成功编译并运行，展示了如何在Rust中定义和调用简单的无参数、无返回值的函数。

## 扩展知识点与解答：

深入探讨`functions1`练习后，我们可以扩展到关于Rust中函数更广泛的概念、使用场景以及如何有效地利用函数提高代码的组织性和复用性。

### 扩展知识点：

1. **函数签名**：
   - 函数签名定义了函数的名称、参数类型和数量以及返回类型。在Rust中，明确的函数签名有助于编译器检查和推断，确保函数的正确使用。

2. **返回值**：
   - 函数可以通过在箭头（`->`）后指定类型来返回值。如果函数执行完毕没有显式返回值，它会隐式返回单元类型`()`。

3. **参数传递**：
   - 函数参数按值（value）、引用（`&`）或可变引用（`&mut`）传递。理解这些不同的传递方式对于编写有效和安全的Rust代码至关重要。

4. **函数作为值**：
   - Rust支持高阶函数，即可以将函数作为参数传递给其他函数，或将函数作为另一个函数的返回值。这增加了Rust编程的灵活性和表达力。

### 扩展解题方法：

- **探索返回值和参数**：
  - 尝试定义一个接受参数并返回值的函数。通过这样的练习，你可以更好地理解函数如何与其他部分的Rust代码交互。

- **代码示例（带参数和返回值的函数）**：
    

```rust
fn main() {
    let number = 5;
    let result = add_one(number);
    println!("{} plus 1 is {}", number, result);
}

// 定义一个接受i32类型参数并返回i32类型的函数
fn add_one(x: i32) -> i32 {
    x + 1 // Rust中最后一个表达式的值会被自动返回，因此不需要`return`关键字
}
```
这个示例中，`add_one`函数接受一个`i32`类型的参数并返回参数值加1的结果。在`main`函数中，我们调用`add_one`并打印结果。这展示了如何定义和使用带参数和返回值的函数。
