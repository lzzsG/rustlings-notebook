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

### 本题内容：

这个练习主要目的是让学生理解和实践 Rust 的移动语义（Move Semantics）以及可变性（Mutability）。学生将通过修正代码错误来学习如何使变量可变，并探索 Rust 中值如何从一个变量移动到另一个变量，特别是在函数参数和返回值的上下文中。注意：该练习特别强调了在函数调用中变量所有权的变化，以及如何通过修改变量的可变性来解决编译时错误。

### 相关知识点：

1. **移动语义（Move Semantics）**：在 Rust 中，当一个值从一个变量移动到另一个变量时，原变量将不再可用。这避免了数据的意外修改和竞态条件。
2. **可变性（Mutability）**：在 Rust 中，默认情况下变量是不可变的。这意味着一旦一个变量被赋值后，它的值就不能改变，除非它被明确声明为可变的。
3. **函数和所有权**：函数可以获取其参数的所有权，也可以将所有权返回给其他变量。理解这一点对于管理 Rust 程序中的数据流至关重要。

### 解题方法：

- **步骤描述**：
  1. 首先识别代码中的错误：尝试修改一个不可变的变量 `vec1`。
  2. 修正错误的关键在于理解错误信息提示的位置可能并不是需要添加修改的地方。
  3. 根据 Rust 的规则，我们需要在声明 `vec0` 变量的地方就指定它为可变的，以便后续可以修改它。

- **代码示例**：

```rust
fn main() {
    // 将 vec0 声明为可变的，这样它就可以被修改了
    let mut vec0 = Vec::new();

    // 由于 vec0 现在是可变的，我们可以将它传递给 fill_vec 函数
    // 这里发生了所有权的移动，vec0 的所有权转移到了 fill_vec 内部
    let vec1 = fill_vec(vec0);

    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);

    // vec0 在这里不再可用，因为它的所有权已经被移动
    // 尝试访问 vec0 会导致编译错误
    // println!("{} has length {} content `{:?}`", "vec0", vec0.len(), vec0);

    // vec1 是可变的，可以正常推入新的值
    // 注意：这里将会引发另一个编译错误，因为 vec1 没有被声明为可变的
    // 要解决这个问题，我们需要将 vec1 声明为可变的
    let mut vec1 = fill_vec(vec0);
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
注意：上面的代码中注释掉的部分展示了尝试访问所有权已经移动的变量会导致的错误。同时，修正方法包括在 `main` 函数中声明 `vec1` 为可变的，以便可以向它推入新的值。

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
