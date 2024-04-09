# Exercise 11

- Name: ```functions4```
- Path: ```exercises/functions/functions4.rs```
#### Hint: 

The error message points to line 17 and says it expects a type after the`->`. This is where the function's return type should be -- take a look at the `is_even` function for an example!

Also: Did you figure out that, technically, u32 would be the more fitting type for the prices here, since they can't be negative? If so, kudos!


---



```rust,editable
// functions4.rs
//
// This store is having a sale where if the price is an even number, you get 10
// Rustbucks off, but if it's an odd number, it's 3 Rustbucks off. (Don't worry
// about the function bodies themselves, we're only interested in the signatures
// for now. If anything, this is a good way to peek ahead to future exercises!)
//
// Execute `rustlings hint functions4` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

fn main() {
    let original_price = 51;
    println!("Your sale price is {}", sale_price(original_price));
}

fn sale_price(price: i32) -> {
    if is_even(price) {
        price - 10
    } else {
        price - 3
    }
}

fn is_even(num: i32) -> bool {
    num % 2 == 0
}

```

---

### 本题内容：

这个练习关注于Rust函数返回值类型的声明。它展示了如何在函数签名中指定返回类型，这对于编译器正确理解函数的行为至关重要。通过修复`sale_price`函数的声明，学生将学习到如何声明具有返回值的函数。

### 相关知识点：

- **函数返回值**：在Rust中，函数可以返回一个值，返回类型通过在函数签名中使用`->`符号后跟类型来指定。如果函数不返回任何值，则不需要`->`和类型。
- **类型推断与显式类型**：虽然Rust有强大的类型推断能力，但函数的返回类型必须显式声明，以确保函数的行为明确无误。

### 解题方法：

- **步骤描述**：
  - 要解决这个问题，我们需要在`sale_price`函数签名中指定一个返回值类型。考虑到`price`是`i32`类型，且函数体中的操作也返回`i32`类型的结果，因此`sale_price`的返回类型应为`i32`。

- **代码示例**：
    ```rust
    // functions4.rs
    // 已完成练习
    
    fn main() {
        let original_price = 51;
        println!("Your sale price is {}", sale_price(original_price));
    }
    
    // 在函数签名中为返回值指定`i32`类型
    fn sale_price(price: i32) -> i32 {
        if is_even(price) {
            price - 10
        } else {
            price - 3
        }
    }
    
    fn is_even(num: i32) -> bool {
        num % 2 == 0
    }
    ```
    在这个修正后的版本中，`sale_price`函数现在明确声明返回一个`i32`类型的值。这样，无论价格是奇数还是偶数，`sale_price`函数都会计算并返回调整后的价格。由于`original_price`被设为51（一个奇数），程序运行时将打印出"Your sale price is 48"。

## 扩展知识点与解答：

深入探讨`functions4`练习之后，我们可以进一步探索Rust中函数返回值的高级特性以及如何有效利用这些特性来编写更灵活和强大的代码。

### 扩展知识点：

1. **多返回值**：
   - Rust函数可以通过元组返回多个值。这允许在单个函数调用中返回复杂的数据结构或多个相关数据。

2. **`Result`和`Option`类型**：
   - Rust提供了`Result`和`Option`枚举来处理可能的错误或空值。使用这些类型作为返回值可以提供更多的上下文信息，并强制调用者处理潜在的错误或空值情况。

3. **隐式返回**：
   - Rust中的函数可以隐式返回其最后一个表达式的值，这意味着在不使用`return`关键字的情况下返回值。这种隐式返回特性使得函数体更简洁。

### 扩展解题方法：

- **利用元组返回多个值**：
  - 当需要从函数返回多个相关值时，可以使用元组。这种方法特别适用于需要同时返回操作结果和状态信息的情况。

- **使用`Result`和`Option`提高错误处理能力**：
  - 在函数可能失败或返回空值的情况下，使用`Result`或`Option`类型作为返回值，而不是仅返回一个可能无法表示错误状态的普通值。

- **代码示例（使用元组返回多个值）**：
    ```rust
    fn divide(dividend: f32, divisor: f32) -> Option<(f32, bool)> {
        if divisor == 0.0 {
            None
        } else {
            Some((dividend / divisor, dividend % divisor == 0.0))
        }
    }
    
    fn main() {
        let result = divide(10.0, 3.0);
        match result {
            Some((quotient, is_exact)) => {
                println!("Quotient: {}", quotient);
                if is_exact {
                    println!("The division is exact.");
                } else {
                    println!("The division is not exact.");
                }
            },
            None => println!("Cannot divide by 0"),
        }
    }
    ```
    这个示例展示了如何使用`Option`枚举和元组从函数返回复杂的结果。`divide`函数计算两个数的除法结果，并返回商以及除法是否精确。通过这种方式，函数的调用者可以更准确地了解操作的结果和状态。
