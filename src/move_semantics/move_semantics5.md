# Exercise 29

- Name: ```move_semantics5```
- Path: ```exercises/move_semantics/move_semantics5.rs```
#### Hint: 

Carefully reason about the range in which each mutable reference is in scope. Does it help to update the value of referent (x) immediately after the mutable reference is taken? 

Read more about 'Mutable References'in the book's section References and Borrowing':
[https://doc.rust-lang.org/book/ch04-02-references-and-borrowing](https://doc.rust-lang.org/book/ch04-02-references-and-borrowing.html?highlight=mutable-references.#mutable-references)  ([Chinese version](https://rustwiki.org/zh-CN/book/ch04-02-references-and-borrowing.html#%E5%8F%AF%E5%8F%98%E5%BC%95%E7%94%A8))



---



```rust,editable
// move_semantics5.rs
//
// Make me compile only by reordering the lines in `main()`, but without adding,
// changing or removing any of them.
//
// Execute `rustlings hint move_semantics5` or use the `hint` watch subcommand
// for a hint.

// I AM NOT DONE

fn main() {
    let mut x = 100;
    let y = &mut x;
    let z = &mut x;
    *y += 100;
    *z += 1000;
    assert_eq!(x, 1200);
}

```

### 本题内容：

这个练习的目标是通过重排代码行来解决编译错误，不需要添加、修改或删除任何代码行。关键在于理解Rust中可变引用的规则，特别是同一作用域内对同一数据的可变引用数量限制。

### 相关知识点：

- **可变引用规则**：在Rust中，你可以有任意数量的不可变引用（`&T`）或恰好一个可变引用（`&mut T`），但这两者不能同时存在。这个规则有助于防止数据竞争，确保内存安全。

- **引用的作用域**：引用的作用域从声明开始，到最后一次使用结束。理解引用的作用域对于合理组织代码以满足可变引用规则非常关键。

### 解题方法：

#### 重排代码行
- **步骤描述**：
  1. 声明并初始化变量`x`。
  2. 创建`x`的第一个可变引用`y`。
  3. 使用`y`修改`x`的值。
  4. **确保`y`的最后一次使用后再创建第二个可变引用`z`**。这是因为Rust不允许在同一作用域内同时存在对同一数据的多个可变引用。
  5. 使用`z`修改`x`的值。
  6. 检查`x`的值是否符合预期。

### 代码示例：

为了符合Rust的借用规则并使程序编译通过，你需要调整代码的顺序，如下所示：

```rust
fn main() {
    let mut x = 100;    // 声明并初始化x
    let y = &mut x;     // 创建x的第一个可变引用y
    *y += 100;          // 使用y修改x的值
    // 注意：在这里，y的作用域结束，因为它之后不再被使用
    let z = &mut x;     // 创建x的第二个可变引用z
    *z += 1000;         // 使用z修改x的值
    assert_eq!(x, 1200); // 检查x的值
}
```

通过这种方式重排代码，`y`的作用域在`z`被创建之前结束，满足了Rust的可变引用规则，允许程序成功编译。

这个练习突出了理解和遵守Rust中的所有权和借用规则的重要性，这是确保Rust程序内存安全的关键。掌握这些规则对于高效地利用Rust编程语言至关重要。

## 扩展知识点与解答：

解决`move_semantics5`练习，特别是通过仅重排代码行的方式，深化了对Rust中可变引用规则和作用域概念的理解。这个练习展示了如何通过控制变量的作用域来遵循Rust的借用规则，下面我们将探讨与此相关的扩展知识点以及进一步的解答方法。

### 扩展知识点：

1. **细粒度的作用域控制**：
   - 在Rust中，可以通过创建新的作用域块（使用花括号`{}`）来限制变量的作用域。这可以帮助管理可变引用的生命周期，从而在不违反借用规则的情况下，对同一变量进行多次可变引用。

2. **引用与数据竞争**：
   - Rust的可变引用规则旨在防止数据竞争，这是并发编程中一个常见的问题。理解这些规则有助于编写既安全又高效的并发代码。

3. **借用检查器（Borrow Checker）**：
   - Rust编译器的借用检查器是确保代码遵守所有权和借用规则的关键工具。深入理解借用检查器的工作原理可以帮助你更好地理解编译器错误和警告。

### 扩展解题方法：

- **利用作用域块控制引用生命周期**：
  - 通过在代码中显式创建额外的作用域块，可以细粒度地控制引用的生命周期，使得对同一变量的多次可变引用成为可能。

#### 代码示例（细粒度作用域控制）：
```rust
fn main() {
    let mut x = 100;
    
    // 使用作用域块限制y的生命周期
    {
        let y = &mut x;
        *y += 100;
    } // y的作用域在这里结束

    // 因为y的作用域已经结束，可以再次创建x的可变引用
    {
        let z = &mut x;
        *z += 1000;
    } // z的作用域在这里结束
    
    assert_eq!(x, 1200);
}
```

这个示例中，通过使用作用域块来限制`y`和`z`的生命周期，允许我们在不违反Rust借用规则的前提下，对`x`进行了两次可变引用。虽然在`move_semantics5`的原始题目中不需要添加或删除代码行，理解作用域块的使用可以帮助你解决更复杂的借用问题。
