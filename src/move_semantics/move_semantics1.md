# Exercise 25

- Name: ```move_semantics1```
- Path: ```exercises/move_semantics/move_semantics1.rs```
#### Hint: 

So you've got the "cannot borrow immutable local variable `vec1` as mutable" error on line 13,
right? The fix for this is going to be adding one keyword, and the addition is NOT on line 13
where the error is.

Also: Try accessing `vec0` after having called `fill_vec()`. See what happens!


---



```rust,editable
// move_semantics1.rs
//
// Execute `rustlings hint move_semantics1` or use the `hint` watch subcommand
// for a hint.

// I AM NOT DONE

fn main() {
    let vec0 = Vec::new();

    let vec1 = fill_vec(vec0);

    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);

    vec1.push(88);

    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);
}

fn fill_vec(vec: Vec<i32>) -> Vec<i32> {
    let mut vec = vec;

    vec.push(22);
    vec.push(44);
    vec.push(66);

    vec
}

```

---

### 本题内容

这个练习的目标是帮助学生理解 Rust 中的所有权和可变性概念。通过实现一个简单的函数来填充向量（vector），学生将学习如何处理所有权的转移以及可变和不可变借用。

### 相关知识点

1. **所有权（Ownership）**：在 Rust 中，每个值都有一个称为其“所有者”的变量。值有且只有一个所有者。当所有者超出作用域时，该值将被丢弃。
2. **移动语义（Move Semantics）**：当一个值从一个变量移动到另一个变量时，原始变量将不能再被使用，除非这种类型实现了 Copy trait。
3. **可变性（Mutability）**：变量默认是不可变的。要修改变量，需要使用 `mut` 关键字显式声明为可变。

### 解题方法

#### 步骤描述：

1. **修复所有权问题**：当你尝试在 `main` 函数中使用 `vec0` 传递给 `fill_vec` 函数时，`vec0` 的所有权被移动到 `fill_vec`。因此，`vec0` 在传递后将不再有效。需要考虑是否要在 `fill_vec` 之后继续使用 `vec0`，如果是，则可能需要克隆或以其他方式处理所有权问题。
2. **处理可变性错误**：在 `main` 函数中，尝试对 `vec1` 进行修改，但 `vec1` 是不可变的。根据提示，我们需要在某处添加 `mut` 关键字来允许对 `vec1` 进行修改。

#### 代码示例：

以下是修改后的 `move_semantics1.rs` 文件：

```rust
fn main() {
    let mut vec0 = Vec::new();  // 此处添加 `mut` 使得 `vec0` 可修改

    let mut vec1 = fill_vec(vec0);  // 添加 `mut` 以允许后续对 `vec1` 的修改

    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);

    vec1.push(88);  // 现在可以修改 `vec1` 了

    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);
}

fn fill_vec(mut vec: Vec<i32>) -> Vec<i32> {  // 在参数 `vec` 上使用 `mut`，允许修改
    vec.push(22);
    vec.push(44);
    vec.push(66);

    vec
}
```

### 说明：

- 在 `main` 函数中，通过添加 `mut` 关键字，我们让 `vec0` 和 `vec1` 变为可修改。这允许我们向 `vec1` 中添加更多的元素。
- 在 `fill_vec` 函数中，参数 `vec` 声明为可变，这允许我们向其添加元素。

这种修改充分体现了如何通过理解和操作 Rust 中的所有权和可变性来解决实际的编程问题。

## 扩展知识点与解答：

### 扩展知识点：

1. **可变借用（Mutable Borrowing）**：
   - 在 Rust 中，你可以通过可变借用来临时修改某个值，而不需要获取它的所有权。这是通过使用引用和借用规则来实现的。可变借用允许你在特定作用域内修改变量，而不改变其所有权状态。

2. **生命周期（Lifetimes）**：
   - 生命周期是 Rust 的一个核心概念，它帮助保证引用有效性。当我们通过引用传递变量时，Rust 需要确保这些引用在使用它们的整个作用域内都是有效的。生命周期注解告诉 Rust 引用何时是有效的，以避免悬垂引用等问题。

3. **克隆（Cloning）**：
   - 当你想要保留数据的所有权并同时传递一个值的副本时，可以使用克隆。克隆是一种昂贵的操作，因为它会在内存中创建值的完整副本。在性能敏感的应用中，考虑使用借用而不是克隆，以避免不必要的性能开销。

4. **Copy trait**：
   - 在 Rust 中，某些简单值类型实现了 `Copy` trait，这意味着它们在被赋值或作为函数参数传递时，原始数据会自动被复制，而不是移动。这对于像整数这样的基本类型非常有用，但不能用于所有权管理更为复杂的类型，如 `Vec`。

### 扩展解题方法：

考虑到上面的扩展知识点，我们可以在原问题基础上探讨其他解决方案：

- 使用可变借用来避免所有权的转移：
```rust
fn main() {
    let mut vec0 = Vec::new();
    fill_vec(&mut vec0); // 传递 vec0 的可变引用

    println!("{} has length {} content `{:?}`", "vec0", vec0.len(), vec0);
    vec0.push(88);
    println!("{} has length {} content `{:?}`", "vec0", vec0.len(), vec0);
}

// 接受一个对 Vec<i32> 的可变引用
fn fill_vec(vec: &mut Vec<i32>) {
    vec.push(22);
    vec.push(44);
    vec.push(66);
}
```
这里通过修改 `fill_vec` 函数签名来接受一个对 `Vec<i32>` 的可变引用，避免了所有权的转移。这样 `vec0` 在 `fill_vec` 被调用之后仍然可用，且可以被修改。

- 生命周期和克隆的应用通常在处理引用和需要维持数据副本的复杂场景中才会涉及，因此在本题的简单上下文中可能不会直接应用。但了解这些概念对于深入理解 Rust 的内存管理模型是非常有益的。
