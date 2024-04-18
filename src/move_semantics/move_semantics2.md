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

### 本题内容

本练习的目标是进一步加深对 Rust 的所有权、借用和可变性概念的理解。通过解决与变量重用和修改相关的问题，学生可以学习如何在 Rust 中合理地管理内存和数据访问，特别是在函数调用中如何传递和返回数据。

### 相关知识点

1. **所有权与移动**：在 Rust 中，当一个变量被传递到一个函数时，其所有权可能被移交到函数中，导致原始变量在调用后无法再次使用。
2. **借用（Borrowing）**：Rust 允许通过引用来“借用”变量，这不会移动所有权。借用分为可变借用和不可变借用，根据是否需要修改借用的数据来选择。
3. **可变性（Mutability）**：可通过 `mut` 关键字声明变量为可变，允许修改其内容。

### 解题方法

#### 步骤描述：

1. **分析现有代码错误**：根据题目中的代码，尝试执行程序会遇到 "borrow of moved value" 的错误。这是因为 `vec0` 被移动到 `fill_vec` 函数中，之后再试图访问 `vec0` 会发生错误。
2. **选择合适的解决方案**：根据提示，有三种主要的解决方法：复制数据、借用数据、修改函数以直接操作传入的引用。

#### 代码示例：

**选择一：使用克隆**

这种方法中，我们通过克隆 `vec0` 并传递克隆体到函数，保留原始向量的使用权。

```rust
fn main() {
    let vec0 = Vec::new();
    let vec0_clone = vec0.clone(); // 克隆 vec0

    let mut vec1 = fill_vec(vec0_clone); // 传递克隆体

    println!(
        "{} has length {}, with contents: `{:?}`",
        "vec0",
        vec0.len(),
        vec0
    );

    vec1.push(88);

    println!(
        "{} has length {}, with contents `{:?}`",
        "vec1",
        vec1.len(),
        vec1
    );
}

fn fill_vec(vec: Vec<i32>) -> Vec<i32> {
    let mut vec = vec;

    vec.push(22);
    vec.push(44);
    vec.push(66);

    vec
}
```

**选择二：让 `fill_vec` 借用参数并在函数内部复制数据**

在这个解决方案中，`fill_vec` 将接收一个不可变的借用（即普通引用），然后在函数内部对这个借用的数据进行克隆。这样做的目的是为了在不改变原始向量的基础上创建并返回一个新的向量。

步骤描述：

1. **更改 `fill_vec` 的参数**：改为接收不可变引用而非所有权。
2. **在函数内部进行克隆**：使用传入向量的克隆来创建新向量并填充数据。
3. **返回新的向量**：函数结束时返回这个新创建的向量。

```rust
fn main() {
    let vec0 = Vec::new();

    let vec1 = fill_vec(&vec0);  // 向 fill_vec 传递 vec0 的不可变引用

    println!(
        "{} has length {}, with contents: `{:?}`",
        "vec0",
        vec0.len(),
        vec0
    );

    println!(
        "{} has length {}, with contents `{:?}`",
        "vec1",
        vec1.len(),
        vec1
    );
}

fn fill_vec(vec: &Vec<i32>) -> Vec<i32> {  // 接收一个不可变引用
    let mut new_vec = vec.clone();  // 克隆传入的向量

    new_vec.push(22);
    new_vec.push(44);
    new_vec.push(66);

    new_vec  // 返回新创建的向量
}
```

这种方法通过借用并克隆原始数据来避免了所有权的问题，同时保持了 `vec0` 和 `vec1` 的独立性。`fill_vec` 不直接修改传入的向量，而是创建了一个新的向量并返回。这种方法有助于保持原始数据的不变性，同时提供一个修改后的数据副本。这种策略适用于那些需要保留原始数据同时还要操作数据副本的情况。

**选择三：直接修改可变借用的引用**

这种方法涉及到对 `fill_vec` 函数进行更改，使其接收一个对 `vec0` 的可变引用，并在函数内部直接对其进行修改。这种方式不需要返回修改后的向量，因为传入的向量已经在原地被修改。由于 `vec1` 会变得多余，可以从代码中移除。

步骤描述：

1. **更改函数签名**：将 `fill_vec` 的参数改为可变引用，不再返回任何内容。
2. **在 `main` 函数中修改调用方式**：传递 `vec0` 的可变引用到 `fill_vec`。
3. **更新 `println!` 输出**：移除对 `vec1` 的引用，确保所有打印语句都使用 `vec0`。

```rust
fn main() {
    let mut vec0 = Vec::new();

    fill_vec(&mut vec0);  // 将 vec0 的可变引用传递给 fill_vec

    println!(
        "{} has length {}, with contents: `{:?}`",
        "vec0",
        vec0.len(),
        vec0
    );

    vec0.push(88);  // 在 `main` 中继续向 vec0 添加元素

    println!(
        "{} has length {}, with contents `{:?}`",
        "vec0",
        vec0.len(),
        vec0
    );
}

fn fill_vec(vec: &mut Vec<i32>) {  // 接收一个可变引用并在函数内修改
    vec.push(22);
    vec.push(44);
    vec.push(66);
    // 不需要返回，因为我们直接修改了传入的向量
}
```

这种方法允许函数直接修改传入的向量，无需进行复制或移动操作。由于向量是在原地修改的，这种方式非常高效。这也展示了 Rust 中通过可变借用来在函数内部直接操作数据的能力，同时避免了所有权的复杂问题。

此解决方案的关键优点在于其效率和简洁性，避免了不必要的数据复制或重复声明变量，使代码更加简洁和高效。

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
