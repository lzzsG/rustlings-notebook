# Exercise 23

- Name: ```vecs1```
- Path: ```exercises/vecs/vecs1.rs```
#### Hint: 

In Rust, there are two ways to define a Vector.
1. One way is to use the `Vec::new()` function to create a new vector
     and fill it with the `push()` method.
2. The second way, which is simpler is to use the `vec![]` macro and
     define your elements inside the square brackets.

Check this chapter: [https://doc.rust-lang.org/stable/book/ch08-01-vectors.html](https://doc.rust-lang.org/stable/book/ch08-01-vectors.html) ([Chinese version](https://rustwiki.org/zh-CN/book/ch08-01-vectors.html))
of the Rust book to learn more.



---



```rust,editable
// vecs1.rs
//
// Your task is to create a `Vec` which holds the exact same elements as in the
// array `a`.
//
// Make me compile and pass the test!
//
// Execute `rustlings hint vecs1` or use the `hint` watch subcommand for a hint.

// I AM NOT DONE

fn array_and_vec() -> ([i32; 4], Vec<i32>) {
    let a = [10, 20, 30, 40]; // a plain array
    let v = // TODO: declare your vector here with the macro for vectors

    (a, v)
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_array_and_vec_similarity() {
        let (a, v) = array_and_vec();
        assert_eq!(a, v[..]);
    }
}

```

---

### 本题内容：

这个练习的目标是让学生了解如何在Rust中创建和使用向量（`Vec`）。特别是，学生将学习两种创建向量的方法，并将它们与数组进行对比。任务是创建一个与给定数组`a`拥有相同元素的向量。

### 相关知识点：

- **向量（`Vec`）**：向量是Rust中一个可增长的数组类型，可以存储相同类型的多个值。与固定大小的数组不同，向量的大小可以在运行时改变。

- **创建向量**：
  1. 使用`Vec::new()`函数创建一个空向量，然后通过`push()`方法向其添加元素。
  2. 使用`vec![]`宏直接在方括号内声明向量的初始元素，这是一种更简洁的创建向量并初始化的方法。

### 解题方法：

- **步骤描述**：
  1. 声明一个数组`a`，包含四个整型元素。
  2. 使用`vec![]`宏创建一个向量`v`，使其包含与数组`a`相同的元素。
  3. 返回一个元组，包含数组`a`和向量`v`。

- **代码示例**：
    

```rust
// vecs1.rs
// 已完成练习

fn array_and_vec() -> ([i32; 4], Vec<i32>) {
    let a = [10, 20, 30, 40]; // a plain array
    // 使用vec!宏直接声明向量的元素
    let v = vec![10, 20, 30, 40]; // 声明一个包含相同元素的向量

    (a, v)
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_array_and_vec_similarity() {
        let (a, v) = array_and_vec();
        assert_eq!(a, v[..]); // 比较数组和向量的元素是否相同
    }
}
```
在这个修正后的版本中，我们通过`vec![]`宏创建了一个向量`v`，并使其包含与数组`a`相同的元素。然后我们返回了一个元组`(a, v)`，并在测试中验证了数组和向量是否包含相同的元素。

通过解决这个练习，你将对Rust中向量的基本使用有了初步的了解，包括如何创建和初始化向量。向量是Rust中处理动态序列数据的重要工具，学会如何有效使用它们对于编写Rust程序来说至关重要。

## 扩展知识点与解答：

完成`vecs1`练习之后，你已经对如何在Rust中创建和初始化向量有了基础的了解。向量是Rust中非常强大的工具，它们提供了比数组更多的灵活性和功能。以下是与向量相关的一些扩展知识点和编码技巧，它们可以帮助你更深入地理解和使用向量。

### 扩展知识点：

1. **向量的动态性**：
   - 向量可以在运行时动态地增长或缩小，这通过`push`和`pop`方法实现。这种动态性使向量成为处理可变长度数据集的理想选择。

2. **向量与迭代器**：
   - 向量与迭代器紧密相关，你可以使用迭代器对向量进行遍历、筛选、变换等操作。Rust中的迭代器是惰性的，这意味着它们在需要产生值时才进行计算。

3. **向量切片**：
   - 与数组切片类似，向量切片允许你引用向量的一部分，而无需复制其中的数据。这在处理向量的子序列时非常有用。

### 扩展解题方法：

- **探索向量的容量和重置**：
  - 向量有容量的概念，即它可以存储的元素数量。了解如何通过`with_capacity`方法预分配向量空间，以及如何使用`clear`方法重置向量，可以帮助你编写更高效的代码。

- **应用向量的迭代器**：
  - 练习使用向量的迭代器进行各种集合操作，如`map`、`filter`、`fold`等。这些操作可以极大地简化对数据集的处理。

- **操作向量切片**：
  - 尝试从向量中创建切片，并使用它们进行操作。理解切片如何工作，以及它们如何与向量的动态性相结合。

- **代码示例（向量迭代器和切片）**：
    

```rust
fn main() {
    let vec = vec![1, 2, 3, 4, 5];
    let even_numbers: Vec<i32> = vec.iter().filter(|&x| x % 2 == 0).cloned().collect();
    println!("Even numbers: {:?}", even_numbers);

    let slice = &vec[1..4];
    println!("Slice of vec: {:?}", slice);
}
```
这个示例展示了如何使用向量的迭代器来筛选出偶数，并将结果收集到一个新的向量中。此外，还演示了如何创建一个向量切片，并打印它的内容。
