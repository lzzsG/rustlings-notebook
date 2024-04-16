# Exercise 75

- Name: ```iterators4```
- Path: ```exercises/iterators/iterators4.rs```
#### Hint: 

In an imperative language, you might write a for loop that updates a mutable variable. Or, you might write code utilizing recursion and a match clause. In Rust you can take another functional approach, computing the factorial elegantly with ranges and iterators.

Hint 2: Check out the `fold` and `rfold` methods!


---



```rust,editable
// iterators4.rs
//
// Execute `rustlings hint iterators4` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

pub fn factorial(num: u64) -> u64 {
    // Complete this function to return the factorial of num
    // Do not use:
    // - return
    // Try not to use:
    // - imperative style loops (for, while)
    // - additional variables
    // For an extra challenge, don't use:
    // - recursion
    // Execute `rustlings hint iterators4` for hints.
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn factorial_of_0() {
        assert_eq!(1, factorial(0));
    }

    #[test]
    fn factorial_of_1() {
        assert_eq!(1, factorial(1));
    }
    #[test]
    fn factorial_of_2() {
        assert_eq!(2, factorial(2));
    }

    #[test]
    fn factorial_of_4() {
        assert_eq!(24, factorial(4));
    }
}

```

---

### 本题内容

本练习的目标是通过使用 Rust 的迭代器和 `fold` 方法来计算一个数的阶乘，这是一种体现函数式编程风格的方法。这种方式避免了传统的循环和递归，展示了如何利用 Rust 强大的迭代器抽象进行紧凑且表达力强的数值计算。

### 相关知识点

1. **阶乘定义**：
   - 阶乘表示为 `n!`，是所有正整数从 1 到 `n` 的乘积，特别地，`0!` 定义为 1。

2. **迭代器 `fold` 方法**：
   - `fold` 是一个将迭代器中的所有元素折叠（或累积）成单一结果的方法。它接受两个参数：一个初始累加器值和一个二元操作函数。

3. **函数式编程风格**：
   - 函数式编程强调无副作用的函数和数据的不可变性。在 Rust 中，`fold` 方法允许以不可变的方式累积值。

### 解题方法

#### 步骤描述：

1. **初始化范围**：
   - 使用 `1..=num` 创建一个包含从 1 到 `num` 的所有整数的迭代器。

2. **使用 `fold` 方法计算阶乘**：
   - `fold` 的初始值设为 1（阶乘的恒等值），通过一个闭包函数来将累加器与当前元素相乘。

3. **处理特殊情况**：
   - 阶乘的定义中，`0!` 是特殊的，等于 1。由于迭代 `1..=0` 是空的，`fold` 将直接返回初始值。

#### 代码示例：

```rust
pub fn factorial(num: u64) -> u64 {
    (1..=num).fold(1, |acc, x| acc * x)
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn factorial_of_0() {
        assert_eq!(1, factorial(0));  // 空迭代，fold返回初始值1
    }

    #[test]
    fn factorial_of_1() {
        assert_eq!(1, factorial(1));  // 1! = 1
    }
    #[test]
    fn factorial_of_2() {
        assert_eq!(2, factorial(2));  // 2! = 1 * 2
    }

    #[test]
    fn factorial_of_4() {
        assert_eq!(24, factorial(4));  // 4! = 1 * 2 * 3 * 4
    }
}
```

通过上述步骤和代码示例，学生可以学习到如何通过迭代器和 `fold` 方法以纯函数式风格解决问题，这种方法不仅代码简洁，而且易于理解和测试。这种技巧特别适用于处理序列数据的累积或转换任务。

## 扩展知识点与解答：

### 扩展知识点

1. **其他迭代器方法**：
   - **`product`**：这是一个专门用于计算迭代器中所有元素的乘积的方法。对于计算阶乘，`product` 是一个非常直接的替代方法。
   - **`scan`**：这个方法可以生成一个迭代器，它持续地应用一个函数，将每次的结果作为下一次的输入，类似于 `fold`，但它返回每一步的中间结果，而不是最终累积值。

2. **并行处理**：
   - 使用 **Rayon** 这样的并行迭代器库可以加速阶乘计算，特别是对于非常大的数字。Rayon 为迭代器提供了并行处理能力，可以在多个线程上分布计算负载。

3. **惰性求值**：
   - Rust 的迭代器本质上是惰性求值的，这意味着计算只有在需要结果的时候才执行。这种方法非常有效，尤其是在处理潜在无限序列或非常大的数据集时。

### 扩展解题方法

1. **使用 `product` 方法计算阶乘**：
   - 这是一个更简洁的方法，直接使用迭代器的 `product` 方法来计算阶乘，避免了显式使用 `fold`。

   ```rust
   pub fn factorial(num: u64) -> u64 {
       (1..=num).product()
   }
   ```

2. **使用 `scan` 方法跟踪阶乘的中间结果**：
   - 可以使用 `scan` 来跟踪计算阶乘过程中的每一个中间结果，这对于教学和调试非常有用。

   ```rust
   pub fn factorial_steps(num: u64) -> Vec<u64> {
       (1..=num).scan(1, |state, x| {
           *state *= x;
           Some(*state)
       }).collect()
   }
   ```

3. **并行计算阶乘**：
   - 如果需要计算非常大的数字的阶乘，可以使用并行处理来提高性能。以下示例假设使用了 Rayon 库：

   ```rust
   use rayon::prelude::*;
   
   pub fn factorial_parallel(num: u64) -> u64 {
       (1..=num).into_par_iter().product()
   }
   ```

通过这些扩展方法，可以更深入地了解和利用 Rust 的迭代器和并行处理能力。这些方法不仅提高了代码的效率和简洁性，而且展示了 Rust 在处理复杂数据操作时的强大功能。
