# Exercise 16

- Name: ```quiz1```
- Path: ```exercises/quiz1.rs```
#### Hint: 

No hints this time ;)


---



```rust,editable
// quiz1.rs
//
// This is a quiz for the following sections:
// - Variables
// - Functions
// - If
//
// Mary is buying apples. The price of an apple is calculated as follows:
// - An apple costs 2 rustbucks.
// - If Mary buys more than 40 apples, each apple only costs 1 rustbuck!
// Write a function that calculates the price of an order of apples given the
// quantity bought. No hints this time!
//
// No hints this time ;)

// I AM NOT DONE

// Put your function here!
// fn calculate_price_of_apples {

// Don't modify this function!
#[test]
fn verify_test() {
    let price1 = calculate_price_of_apples(35);
    let price2 = calculate_price_of_apples(40);
    let price3 = calculate_price_of_apples(41);
    let price4 = calculate_price_of_apples(65);

    assert_eq!(70, price1);
    assert_eq!(80, price2);
    assert_eq!(41, price3);
    assert_eq!(65, price4);
}

```

---

### 本题内容：

这个练习是关于变量、函数、以及`if`语句的一个综合测试。目标是编写一个函数，根据购买的苹果数量计算总价。苹果的价格根据购买数量有不同的折扣规则。

### 解题方法：

- **步骤描述**：
  1. 定义一个函数`calculate_price_of_apples`，接受购买的苹果数量作为参数，并返回总价。
  2. 根据题目描述的价格规则，使用`if`表达式来判断购买的苹果数量。
  3. 如果购买的苹果数量超过40，则每个苹果的价格为1 rustbuck；否则，价格为2 rustbucks。
  4. 计算并返回苹果的总价。

- **代码示例**：
    

```rust
// 定义计算苹果总价的函数
fn calculate_price_of_apples(quantity: u32) -> u32 {
    if quantity > 40 {
        quantity * 1 // 购买数量超过40，每个苹果1 rustbuck
    } else {
        quantity * 2 // 购买数量不超过40，每个苹果2 rustbucks
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn verify_test() {
        let price1 = calculate_price_of_apples(35);
        let price2 = calculate_price_of_apples(40);
        let price3 = calculate_price_of_apples(41);
        let price4 = calculate_price_of_apples(65);

        assert_eq!(70, price1);
        assert_eq!(80, price2);
        assert_eq!(41, price3);
        assert_eq!(65, price4);
    }
}
```
这段代码定义了`calculate_price_of_apples`函数，它根据购买数量来计算苹果的总价，并能通过提供的单元测试。

### 单元测试详解：

- 单元测试通过`#[cfg(test)]`属性来定义一个测试模块，确保这些测试代码只在执行`cargo test`时编译和运行。
- `#[test]`属性标记单个测试函数。每个测试函数独立运行，验证特定的功能或行为。
- `assert_eq!`宏用于比较期望值和实际值。如果两者不相等，测试失败并报告不匹配。

通过解决这个练习，你不仅复习了之前学习的概念，还练习了如何根据具体需求设计函数，并验证其正确性通过编写和运行单元测试。

## 扩展知识点与解答：

通过完成`quiz1`练习，我们可以进一步探索Rust中的一些高级主题和编码技巧，以及如何利用单元测试确保代码的健壮性和正确性。

### 扩展知识点：

1. **泛型和类型安全**：
   - 考虑到不同场景可能需要处理不同类型的数值，使用泛型可以使函数更加通用和灵活。Rust的强类型系统和泛型支持能够帮助开发者编写既安全又通用的代码。

2. **错误处理**：
   - 在实际应用中，处理潜在的错误是不可避免的。Rust提供了`Result`和`Option`类型来帮助开发者以类型安全的方式处理可能的错误和空值。

3. **性能优化**：
   - 了解何时使用栈分配的类型（如基本数值类型和元组）与何时使用堆分配的类型（如`Vec`和`Box`）对于优化程序性能至关重要。

### 扩展解题方法：

- **考虑输入范围和类型**：
  - 当设计函数时，考虑输入的可能范围和类型。例如，如果函数只应接受正数，那么使用无符号整数类型（如`u32`）是一个好的选择。

- **使用单元测试进行边界值测试**：
  - 在编写单元测试时，不仅要测试典型情况，还要测试边界值和异常情况，以确保函数在各种输入下都能正常工作。

- **代码示例（考虑使用泛型）**：
    

```rust
// 假设的泛型函数示例，这里仅为展示泛型用法，并非直接应用于当前练习
fn calculate_value<T: std::ops::Add<Output = T> + From<u8>>(quantity: T) -> T {
    quantity + T::from(2) // 假设的计算，加上2
}
```

- **单元测试的边界值测试**：


```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_zero_quantity() {
        assert_eq!(0, calculate_price_of_apples(0));
    }

    #[test]
    fn test_negative_quantity() {
        // 假设函数可以处理负数输入
        // 这需要函数支持错误处理或使用有符号类型
    }
}
```
这些测试示例强调了测试典型情况外的边界值的重要性。虽然`calculate_price_of_apples`函数不直接处理负数（由于使用了`u32`），但考虑这类边界情况对于编写健壮的代码非常重要。
