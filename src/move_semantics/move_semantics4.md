# Exercise 28

- Name: ```move_semantics4```
- Path: ```exercises/move_semantics/move_semantics4.rs```
#### Hint: 

Stop reading whenever you feel like you have enough direction :) Or try doing one step and then fixing the compiler errors that result! So the end goal is to:
   - get rid of the first line in main that creates the new vector
   - so then `vec0` doesn't exist, so we can't pass it to `fill_vec`
   - `fill_vec` has had its signature changed, which our call should reflect
   - since we're not creating a new vec in `main` anymore, we need to create
     a new vec in `fill_vec`, similarly to the way we did in `main`


---



```rust,editable
// move_semantics4.rs
//
// Refactor this code so that instead of passing `vec0` into the `fill_vec`
// function, the Vector gets created in the function itself and passed back to
// the main function.
//
// Execute `rustlings hint move_semantics4` or use the `hint` watch subcommand
// for a hint.

// I AM NOT DONE

fn main() {
    let vec0 = Vec::new();

    let mut vec1 = fill_vec(vec0);

    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);

    vec1.push(88);

    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);
}

// `fill_vec()` no longer takes `vec: Vec<i32>` as argument
fn fill_vec() -> Vec<i32> {
    let mut vec = vec;

    vec.push(22);
    vec.push(44);
    vec.push(66);

    vec
}

```

---

### 本题内容：

本练习的目标是重构现有代码，使`fill_vec`函数不再接受向量作为参数，而是在函数内部创建新的向量，并将其返回给调用者。这要求修改`fill_vec`的签名和实现，以及调整`main`函数中对`fill_vec`的调用方式。

### 相关知识点：

- **向量的创建**：在Rust中，可以使用`Vec::new()`或`vec![]`宏来创建新的向量。

- **函数返回值**：函数可以返回值给调用者。在这种情况下，`fill_vec`函数将创建并返回一个`Vec<i32>`类型的向量。

- **所有权**：在Rust中，返回值会将所有权从函数内部转移给调用者。

### 解题方法：

#### 调整`fill_vec`函数
- **步骤描述**：
  1. 修改`fill_vec`函数，移除它的参数列表。
  2. 在`fill_vec`函数内部，创建一个新的向量。
  3. 向这个新创建的向量添加元素。
  4. 返回这个向量。

#### 修改`main`函数调用
- **步骤描述**：
  1. 移除`main`函数中创建`vec0`向量的代码行。
  2. 直接调用`fill_vec`函数，并将返回的向量赋值给`vec1`。

### 代码示例：

```rust
fn main() {
    // 移除vec0的创建，直接调用fill_vec
    let mut vec1 = fill_vec();

    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);

    vec1.push(88);

    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);
}

fn fill_vec() -> Vec<i32> {
    // 在函数内部创建新的向量
    let mut vec = Vec::new();

    vec.push(22);
    vec.push(44);
    vec.push(66);

    vec
}
```

通过这种方式，我们将向量的创建逻辑从`main`函数转移到了`fill_vec`函数内部，这使得代码更加模块化，并且`fill_vec`函数的复用性也更强。这个练习强调了Rust所有权和函数返回值的概念，同时展示了如何通过函数返回值来传递数据。掌握这些概念对于理解Rust的内存安全模型和编写高质量的Rust代码非常重要。

### 扩展知识点：

1. **所有权和函数**：
   - Rust的所有权规则对于理解函数如何与数据交互至关重要。函数可以获取数据的所有权、借用数据（可变或不可变），或者创建新数据并返回所有权给调用者。理解这些交互模式对于编写有效和安全的Rust代码是必要的。

2. **数据封装**：
   - 在函数内部创建并返回新的数据结构是一种封装数据逻辑的好方法。这种模式可以帮助你将数据创建逻辑与应用的其他部分分离，从而提高代码的可维护性和可读性。

3. **借用和可变性**：
   - 尽管这个练习主要关注在函数内创建和返回向量，但在实际应用中，函数经常需要借用数据进行读取或修改。理解Rust中的借用规则（包括可变借用和不可变借用）对于安全地管理数据访问和修改同样重要。

### 扩展解答方法：

- **返回多个值**：
  - Rust函数可以通过元组返回多个值。这在你需要从函数返回多个相关的数据时非常有用。例如，一个函数可能会处理向量并返回处理后的向量及其长度。

- **使用结构体封装复杂数据**：
  - 当函数需要处理和返回复杂数据时，使用结构体来封装这些数据是一个好选择。结构体可以提供更清晰的语义和更强的类型安全性。

- **利用枚举处理多种结果**：
  - 在一些情况下，函数可能有多种可能的结果。Rust的枚举可以用来封装不同类型的返回值，与`Result`和`Option`类型一起，为错误处理和可选值提供了强大的机制。

### 实践建议：

- **探索标准库中的集合**：
  - Rust标准库提供了多种数据集合类型。深入了解这些类型及其方法，可以帮助你更有效地使用Rust进行数据处理。

- **实现错误处理**：
  - 在返回值的函数中实现错误处理是Rust编程的一个重要方面。利用`Result`类型来表示操作可能成功或失败的结果，可以提高你程序的健壮性和可靠性。
