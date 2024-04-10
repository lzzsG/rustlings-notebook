# Exercise 26

- Name: ```move_semantics2```
- Path: ```exercises/move_semantics/move_semantics2.rs```
#### Hint: 

When running this exercise for the first time, you'll notice an error about "borrow of moved value". In Rust, when an argument is passed to a function andit's not explicitly returned, you can't use the original variable anymore.

We call this "moving" a variable. When we pass `vec0` into `fill_vec`, it's being "moved" into `vec1`, meaning we can't access `vec0` anymore after the fact.
Rust provides a couple of different ways to mitigate this issue, feel free to try them all:

1. You could make another, separate version of the data that's in `vec0` and pass that
   to `fill_vec` instead.
2. Make `fill_vec` borrow its argument instead of taking ownership of it,
   and then copy the data within the function (`vec.clone()`) in order to return an owned
   `Vec<i32>`.
3. Or, you could make `fill_vec` *mutably* borrow a reference to its argument (which will need to be
   mutable), modify it directly, then not return anything. This means that `vec0` will change over the
   course of the function, and makes `vec1` redundant (make sure to change the parameters of the `println!` statements if you go this route)



---



```rust,editable
// move_semantics2.rs
//
// Expected output:
// vec0 has length 3, with contents `[22, 44, 66]`
// vec1 has length 4, with contents `[22, 44, 66, 88]`
//
// Execute `rustlings hint move_semantics2` or use the `hint` watch subcommand
// for a hint.

// I AM NOT DONE

fn main() {
    let vec0 = Vec::new();

    let mut vec1 = fill_vec(vec0);

    println!("{} has length {}, with contents: `{:?}`", "vec0", vec0.len(), vec0);

    vec1.push(88);

    println!("{} has length {}, with contents `{:?}`", "vec1", vec1.len(), vec1);
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

本练习旨在深化对Rust所有权和变量移动（"moving"）概念的理解，特别是当变量作为参数传递给函数时。通过这个练习，学生将遇到因变量移动导致的“borrow of moved value”编译错误，并可以通过几种方法来解决这一问题。

### 相关知识点：

- **变量移动**：在Rust中，当一个变量被作为参数传递给函数时，该变量的所有权会转移到函数中，导致原变量在之后不再可用，除非该值被函数返回。

- **借用**：通过借用，可以让函数访问变量的值而不取得其所有权，分为不可变借用和可变借用。

- **克隆**：通过`.clone()`方法可以复制一份数据的完整副本，这样原变量和复制后的变量就拥有独立的数据，互不影响。

### 解题方法：

#### 方法一：克隆数据
- **步骤描述**：
  1. 在`fill_vec`函数调用前，使用`.clone()`方法创建`vec0`的一个副本，并将这个副本传递给函数。

#### 方法二：函数借用参数
- 在这种方法中，`fill_vec`函数将通过借用参数来访问传入的向量，而不是获取其所有权。这要求我们对函数签名和函数体进行一些调整。

  #### 步骤描述：

  1. 修改`fill_vec`函数签名，使其接受一个向量的不可变引用。由于我们需要返回一个新的向量，函数内部将对传入的向量进行克隆操作。
  2. 在函数体内部，使用`.clone()`方法复制传入向量的副本，然后在这个副本上进行操作。
  3. 返回修改后的向量副本。

#### 方法三：可变借用
- 此方法要求`fill_vec`函数接受一个向量的可变引用，直接在这个向量上进行修改，而不返回任何值。这意味着原始向量`vec0`将在函数调用后被更新。

  #### 步骤描述：

  1. 修改`fill_vec`函数签名，使其接受一个向量的可变引用。
  2. 在函数体内部，直接在传入的向量上进行修改，无需返回值。
  3. 因为`vec0`在函数中直接被修改，所以在函数调用后`vec0`的状态将发生变化。

### 代码示例：

#### 方法一：克隆数据
```rust
fn main() {
    let vec0 = Vec::new();
    let mut vec1 = fill_vec(vec0.clone()); // 克隆vec0

    // 注意：由于vec0已经被移动，以下行会导致编译错误
    // println!("{} has length {}, with contents: `{:?}`", "vec0", vec0.len(), vec0);

    vec1.push(88);

    println!("{} has length {}, with contents `{:?}`", "vec1", vec1.len(), vec1);
}
```

#### 方法二：函数借用参数

```rust
fn main() {
    let vec0 = Vec::new();
    // 注意：此处传递vec0的引用
    let vec1 = fill_vec(&vec0);

    // 注意：此处无法打印vec0的状态，因为vec0并未被修改
    vec1.push(88);

    println!("{} has length {}, with contents `{:?}`", "vec1", vec1.len(), vec1);
}

// 修改函数签名，接受一个对Vec<i32>的不可变引用
fn fill_vec(vec: &Vec<i32>) -> Vec<i32> {
    // 在函数内部，克隆传入的向量
    let mut vec = vec.clone();

    vec.push(22);
    vec.push(44);
    vec.push(66);

    vec
}
```

#### 方法三：可变借用

```rust
fn main() {
    // 注意：此处声明vec0为可变
    let mut vec0 = Vec::new();
    // 传递vec0的可变引用给fill_vec函数
    fill_vec(&mut vec0);

    // vec0现在已经被修改
    println!("{} has length {}, with contents: `{:?}`", "vec0", vec0.len(), vec0);

    vec0.push(88);

    println!("{} has length {}, with contents `{:?}`", "vec0", vec0.len(), vec0);
}

// 修改函数签名，接受一个对Vec<i32>的可变引用
fn fill_vec(vec: &mut Vec<i32>) {
    vec.push(22);
    vec.push(44);
    vec.push(66);
}

```

通过解决这个练习，你将更深入地理解Rust中所有权、变量移动、借用和克隆的概念及其应用。这些是Rust安全内存管理的核心部分，掌握它们对于编写高效且安全的Rust代码至关重要。

解决`move_semantics2`练习不仅加深了对Rust所有权、变量移动、借用和克隆的理解，还触及了Rust编程中的一些核心概念。这些概念是Rust语言安全内存管理策略的基石。让我们探索与本题相关的扩展知识点，以及如何在更广泛的上下文中应用这些知识。

### 扩展知识点：

后续我们讲学习更多相关知识点。

1. **生命周期（Lifetimes）**：
   - 生命周期是Rust用来确保引用有效性的机制。当你开始使用引用传递数据而非所有权传递时，理解生命周期变得尤为重要。生命周期确保了数据在被引用时不会离开作用域而被丢弃。

2. **模式匹配（Pattern Matching）与借用**：
   - Rust的模式匹配是处理数据结构，特别是枚举时的强大工具。结合借用，你可以安全地解构并访问数据结构的内容，而不必担心所有权的问题。

3. **引用计数（Reference Counting）**：
   - 对于需要多个所有者的场景，Rust提供了`Rc<T>`类型。`Rc<T>`，即引用计数类型，使得一个值可以有多个所有者。这在函数间共享数据时非常有用，尤其是当你无法确定哪个所有者会最后离开作用域时。

4. **内部可变性（Interior Mutability）**：
   - Rust通常要求可变性与所有权一致，但`Cell<T>`和`RefCell<T>`等类型通过内部可变性模式，允许你绕过这一规则。这对于在运行时改变那些在编译时被认为是不可变的值特别有用。

5. **智能指针（Smart Pointers）**：
   - 除了`Box<T>`、`Rc<T>`外，Rust还有`Arc<T>`和`Mutex<T>`等类型，它们扩展了所有权和并发编程的概念。学习这些智能指针如何工作，将帮助你编写更高效、更安全的并发代码。

### 应用场景：

- 在需要通过函数共享数据但又不想转移所有权时，了解如何使用引用和借用是非常重要的。
- 当你的应用需要多线程或者数据之间存在复杂的共享关系时，理解`Arc<T>`、`Mutex<T>`等并发工具的工作原理至关重要。
- 在构建大型Rust应用时，合理利用生命周期和智能指针将有助于管理复杂的内存管理场景，确保应用的安全性和效率。
