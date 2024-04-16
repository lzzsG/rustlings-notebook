# Exercise 74

- Name: ```iterators3```
- Path: ```exercises/iterators/iterators3.rs```
#### Hint: 

The divide function needs to return the correct error when even division is notpossible.

The division_results variable needs to be collected into a collection type.

The result_with_list function needs to return a single Result where the success
case is a vector of integers and the failure case is a DivisionError.

The list_of_results function needs to return a vector of results.

See [https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.collect ](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.collect )  ([Chinese version](https://rustwiki.org/zh-CN/std/iter/trait.Iterator.html#method.collect)) for how the `FromIterator` trait is used in `collect()`. This trait is REALLY powerful! It can make the solution to this exercise infinitely easier.


---

```rust,editable
// iterators3.rs
//
// This is a bigger exercise than most of the others! You can do it! Here is
// your mission, should you choose to accept it:
// 1. Complete the divide function to get the first four tests to pass.
// 2. Get the remaining tests to pass by completing the result_with_list and
//    list_of_results functions.
//
// Execute `rustlings hint iterators3` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

#[derive(Debug, PartialEq, Eq)]
pub enum DivisionError {
    NotDivisible(NotDivisibleError),
    DivideByZero,
}

#[derive(Debug, PartialEq, Eq)]
pub struct NotDivisibleError {
    dividend: i32,
    divisor: i32,
}

// Calculate `a` divided by `b` if `a` is evenly divisible by `b`.
// Otherwise, return a suitable error.
pub fn divide(a: i32, b: i32) -> Result<i32, DivisionError> {
    todo!();
}

// Complete the function and return a value of the correct type so the test
// passes.
// Desired output: Ok([1, 11, 1426, 3])
fn result_with_list() -> () {
    let numbers = vec![27, 297, 38502, 81];
    let division_results = numbers.into_iter().map(|n| divide(n, 27));
}

// Complete the function and return a value of the correct type so the test
// passes.
// Desired output: [Ok(1), Ok(11), Ok(1426), Ok(3)]
fn list_of_results() -> () {
    let numbers = vec![27, 297, 38502, 81];
    let division_results = numbers.into_iter().map(|n| divide(n, 27));
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_success() {
        assert_eq!(divide(81, 9), Ok(9));
    }

    #[test]
    fn test_not_divisible() {
        assert_eq!(
            divide(81, 6),
            Err(DivisionError::NotDivisible(NotDivisibleError {
                dividend: 81,
                divisor: 6
            }))
        );
    }

    #[test]
    fn test_divide_by_0() {
        assert_eq!(divide(81, 0), Err(DivisionError::DivideByZero));
    }

    #[test]
    fn test_divide_0_by_something() {
        assert_eq!(divide(0, 81), Ok(0));
    }

    #[test]
    fn test_result_with_list() {
        assert_eq!(format!("{:?}", result_with_list()), "Ok([1, 11, 1426, 3])");
    }

    #[test]
    fn test_list_of_results() {
        assert_eq!(
            format!("{:?}", list_of_results()),
            "[Ok(1), Ok(11), Ok(1426), Ok(3)]"
        );
    }
}

```

---

### 本题内容

在这个练习中，目标是实现一个能够处理除法运算并正确处理错误情况的函数，并使用这个函数来生成两种不同的结果集合。通过这个任务，学生将学习错误处理和迭代器的高级应用，特别是在涉及结果集合和错误传播的情况下。

### 相关知识点

1. **错误处理**：
   - Rust 中使用 `Result` 类型来处理可能出现错误的运算。`Result<T, E>` 有两个变体：`Ok(T)` 表示成功并包含结果，`Err(E)` 表示错误并包含错误信息。

2. **自定义错误类型**：
   - 可以定义自己的错误类型来提供更详细的错误信息。在本例中，使用 `DivisionError` 枚举来表示可能的除法错误。

3. **迭代器的错误处理**：
   - 使用迭代器处理可能失败的操作时，可以生成结果的集合（如 `Vec<Result<T, E>>`）或尝试合并这些结果到单个 `Result` 中（如 `Result<Vec<T>, E>`）。

4. **`collect()` 方法的使用**：
   - `collect()` 方法非常强大，能够将迭代器转换成不同的集合类型。根据目标类型，`collect()` 方法可以从迭代器中生成 `Vec<T>`, `Vec<Result<T, E>>`, 或 `Result<Vec<T>, E>` 等。

### 解题方法

#### 步骤描述：

**步骤 1：实现 `divide` 函数**
- 验证除数不为零，否则返回 `DivisionError::DivideByZero`。
- 检查结果是否可以整除，如果不能，返回 `DivisionError::NotDivisible`，否则返回 `Ok` 结果。

**步骤 2：实现 `result_with_list` 函数**
- 使用 `map` 应用 `divide` 函数。
- 使用 `collect()` 尝试合并结果为一个 `Result<Vec<i32>, DivisionError>`。

**步骤 3：实现 `list_of_results` 函数**
- 类似于步骤2，但使用 `collect()` 来生成一个包含各个结果的向量，即 `Vec<Result<i32, DivisionError>>`。

#### 代码示例：

```rust
// 完成除法函数
pub fn divide(a: i32, b: i32) -> Result<i32, DivisionError> {
    if b == 0 {
        return Err(DivisionError::DivideByZero);
    }
    if a % b != 0 {
        return Err(DivisionError::NotDivisible(NotDivisibleError { dividend: a, divisor: b }));
    }
    Ok(a / b)
}

// 返回单一的 Result 包含向量
fn result_with_list() -> Result<Vec<i32>, DivisionError> {
    let numbers = vec![27, 297, 38502, 81];
    let division_results: Result<Vec<_>, _> = numbers.into_iter().map(|n| divide(n, 27)).collect();
    division_results
}

// 返回向量包含多个 Result
fn list_of_results() -> Vec<Result<i32, DivisionError>> {
    let numbers = vec![27, 297, 38502, 81];
    let division_results: Vec<_> = numbers.into_iter().map(|n| divide(n, 27)).collect();
    division_results
}
```

通过这些步骤和代码示例，学生可以深入了解 Rust 中的错误处理和迭代器应用，以及如何使用 `collect()` 方法灵活地处理集合和错误。这些技能对于构建健壮且高效的 Rust 应用程序至关重要。

### `collect()` 方法如何工作

Rust 的 `collect()` 方法非常灵活，其返回类型可以根据上下文中提供的类型标注自动调整。这种行为是通过 Rust 的类型推断和 `FromIterator` trait 实现的，这使得 `collect()` 方法能够将迭代器转换为多种不同的集合类型。

当你调用 `collect()` 方法时，Rust 编译器会查看期望的返回类型，并尝试找到一种方法，将迭代器中的元素转换为该类型。这是通过 `FromIterator` trait 实现的，该 trait 为不同的集合类型定义了如何从迭代器收集元素。

#### 示例

比如在处理可能失败的操作时，你可能会遇到以下两种情形：

1. **收集到单一 `Result`**：
   如果你想将多个结果合并为一个，当所有操作都成功时返回 `Ok` 包含一个集合，任何操作失败时返回第一个 `Err`，你可以这样使用 `collect()`：

   ```rust
   let results: Result<Vec<_>, _> = iterator.collect();
   ```

   在这个例子中，`collect()` 试图将 `Iterator<Item = Result<T, E>>` 转换为 `Result<Vec<T>, E>`。成功的情况下，所有的 `Ok` 值会被集合起来；一旦遇到 `Err`，则立即停止处理并返回该错误。

2. **收集到结果向量**：
   如果你希望分别处理每个元素的结果，并且保留所有的成功和失败结果，可以这样使用 `collect()`：

   ```rust
   let results: Vec<Result<T, E>> = iterator.collect();
   ```

   这里，`collect()` 将每个 `Result<T, E>` 元素直接收集到一个向量中，不管是成功还是失败的结果。

#### 类型推断的重要性

在实际编码中，Rust 的类型推断系统会根据你提供的目标类型来决定 `collect()` 应该使用哪个具体的 `FromIterator` 实现。如果没有显式提供类型，Rust 编译器可能无法推断出你想要的集合类型，这时你可能需要提供更明确的类型标注。因此，`collect()` 的灵活性和强大功能在很大程度上依赖于 Rust 的类型系统，使其成为处理迭代器数据的非常强大的工具。

###  `collect()` 的用法

`collect()` 是 Rust 中迭代器最强大的工具之一，它可以将迭代器中的元素转换为几乎任何集合类型。这个转换过程完全取决于目标类型，这是通过 Rust 的 `FromIterator` trait 实现的。下面将介绍一些 `collect()` 的扩展用法，展示其灵活性和多样性。

#### 基本用法

1. **收集到 `Vec`**：
   ```rust
   let numbers = (1..5).collect::<Vec<_>>();
   ```

2. **收集到 `HashSet`**：
   ```rust
   use std::collections::HashSet;
   let numbers = vec![1, 2, 2, 3, 4, 4];
   let unique_numbers: HashSet<_> = numbers.into_iter().collect();
   ```

3. **收集到 `HashMap`**：
   ```rust
   use std::collections::HashMap;
   let pairs = vec![("one", 1), ("two", 2)];
   let map: HashMap<_, _> = pairs.into_iter().collect();
   ```

#### 高级用法

4. **使用 `filter_map` 与 `collect` 结合**：
   过滤并转换元素，然后收集结果：
   ```rust
   let strings = vec!["1", "two", "3", "four"];
   let numbers: Vec<i32> = strings
       .into_iter()
       .filter_map(|s| s.parse().ok())
       .collect();
   ```

5. **使用 `flat_map` 与 `collect` 结合**：
   展开嵌套的集合并收集结果：
   ```rust
   let nested_vec = vec![vec![1, 2, 3], vec![4, 5, 6]];
   let flat: Vec<_> = nested_vec.into_iter().flat_map(|x| x.into_iter()).collect();
   ```

6. **收集为 `Result`**：
   如果迭代器中的元素为 `Result` 类型，可以尝试将它们合并成一个 `Result`，只要遇到第一个 `Err` 就停止处理：
   ```rust
   let results = vec![Ok(1), Ok(2), Err("oops"), Ok(4)];
   let res: Result<Vec<_>, _> = results.into_iter().collect();
   ```

7. **收集并转换为字符串**：
   如果元素可以序列化为字符串，可以直接收集为一个单独的字符串：
   ```rust
   let chars = ['h', 'e', 'l', 'l', 'o'];
   let greeting: String = chars.iter().collect();
   ```

#### 使用自定义结构

如果你有一个自定义类型，并希望能够将迭代器直接收集到这种类型中，你可以为该类型实现 `FromIterator` trait：

```rust
struct MyCollection {
    elements: Vec<i32>,
}

impl FromIterator<i32> for MyCollection {
    fn from_iter<I: IntoIterator<Item = i32>>(iter: I) -> Self {
        let mut c = MyCollection { elements: Vec::new() };
        for i in iter {
            c.elements.push(i);
        }
        c
    }
}

let collected: MyCollection = (1..5).collect();
```

这些示例展示了 `collect()` 的多样性和强大功能，能够适应各种数据结构和处理需求，使其成为 Rust 编程中不可或缺的工具。
