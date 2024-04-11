# Exercise 50

- Name: ```options3```
- Path: ```exercises/options/options3.rs```
#### Hint: 

The compiler says a partial move happened in the `match` statement. How can this be avoided? The compiler shows the correction needed. After making the correction as suggested by the compiler, do
read: 

[https://doc.rust-lang.org/std/keyword.ref.html](https://doc.rust-lang.org/std/keyword.ref.html) ([Chinese version](https://rustwiki.org/zh-CN/std/keyword.ref.html))


---

```rust,editable
// options3.rs
//
// Execute `rustlings hint options3` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let y: Option<Point> = Some(Point { x: 100, y: 200 });

    match y {
        Some(p) => println!("Co-ordinates are {},{} ", p.x, p.y),
        _ => panic!("no match!"),
    }
    y; // Fix without deleting this line.
}

```

---

### 本题内容

本练习旨在让学生熟悉Rust的所有权、借用以及`Option`类型的使用。学生将学习如何处理在`match`表达式中因部分移动而导致的编译错误，以及如何使用引用来解决这类问题。

### 相关知识点

- **所有权和借用**：Rust的所有权系统确保内存安全无泄露。所有权的规则包括：每个值都有一个被称为其所有者的变量；值在任一时刻只能有一个所有者；当所有者离开作用域，这个值会被丢弃。借用则允许你有一个对值的引用而不取得其所有权。
- **Option类型**：`Option<T>`类型用于有值(`Some`)或无值(`None`)的场景，是Rust的一种枚举类型，用于处理可能的空值。
- **匹配和引用**：`match`语句允许你根据值的不同可能性来执行不同的代码块。使用`ref`关键字可以在匹配的分支中创建一个值的引用，而不是移动它。

### 解题方法：

在这个具体的例子中，如果你需要在`match`表达式之后保留对`y`的访问，同时遵循原始代码中的注释要求不删除`y;`这一行，解决方案是使用模式匹配时采用借用的方式，即将`y`改为匹配其引用`&y`而不是`y`本身。这样可以避免所有权的转移，允许在`match`之后仍然可以引用`y`。

要实现这一点，首先需要将`match y`修改为`match &y`，随后在`match`的分支中相应地调整匹配模式。具体来说，当`y`是`Some`变体时，使用`Some(ref p)`而不是`Some(p)`来匹配，这样`p`就成了对`Point`的引用。通过这种方式，`y`的所有权不会被移动，因此在`match`表达式之后仍然可以访问`y`。

**代码示例：**

```rust
struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let y: Option<Point> = Some(Point { x: 100, y: 200 });

    match &y {
        Some(ref p) => println!("Co-ordinates are {},{} ", p.x, p.y),
        _ => panic!("no match!"),
    }

    // 由于我们没有移动`y`的所有权，这里对`y`的使用是合法的
    // 例如，可以检查`y`是否是`Some`或`None`
    if y.is_some() {
        println!("y is still available and has a value!");
    } else {
        println!("y is None");
    }
}
```

这段代码中，`ref`关键字用于创建一个指向`y`中值（如果存在）的引用，而不是取得其所有权。这意味着在`match`表达式之后，`y`仍然是有效的，可以被再次访问或使用。

**关键点总结：**

- 当你想在匹配操作中访问 `Option<T>` 的值而不移动它时，使用 `ref` 关键字创建对值的引用。
- 这种方式允许你在 `match` 之后仍然可以访问原始的 `Option<T>` 变量。
- 这个练习展示了 Rust 的所有权和借用规则在实践中的应用，特别是如何在不放弃所有权的情况下从 `Option<T>` 中提取值。

## 扩展知识点与解答：

### 扩展知识点

解决这个练习不仅需要理解`Option`类型和模式匹配的基本使用，还需要深入了解以下几个扩展知识点：

1. **所有权与借用**: Rust的核心特性之一是所有权系统，它通过三个规则来确保内存安全：每个值都有一个变量作为其所有者；每个值在任一时刻只能有一个所有者；当所有者（变量）离开作用域时，值将被丢弃。借用是Rust中的一个机制，允许你通过引用访问数据而不取得其所有权，分为不可变借用和可变借用。

2. **`Option`类型**: `Option<T>`枚举用来表示一个值要么是某个值(`Some`)，要么是无(`None`)。它是Rust的一种解决空值问题的方法，强制程序员在使用值之前必须明确处理空的情况。

3. **匹配引用**: 当你想要在不转移所有权的情况下匹配一个值时，可以匹配该值的引用。这通常通过在`match`表达式中使用`&`符号来实现，同时在模式匹配的分支中使用`ref`关键字来获取值的引用而不是值本身。

4. **模式匹配的进阶用法**: Rust中的模式匹配非常强大，支持多种复杂的匹配模式，包括匹配字面值、解构（结构体、枚举、元组）、忽略值的匹配等。

### 扩展解题方法

了解了上述知识点后，我们可以探索几种不同的方法来解决本题：

1. **使用模式匹配的引用**:
   修改原有的模式匹配，让其匹配`y`的引用而不是`y`本身。这种方式不会转移所有权，允许我们在`match`表达式后继续使用`y`。

2. **使用`if let`结构**:
   作为`match`的简化形式，`if let`可以用于处理只关心一种匹配情况的场景。如果只需要处理`Option`是`Some`的情况，可以使用`if let Some(ref p) = y {...}`来避免部分移动。

3. **克隆`Option`中的值**:
   如果类型实现了`Clone`特质，可以考虑先克隆`Option`中的值，然后对克隆进行操作。这样原始`Option`仍然保持不变，可以继续使用。不过这种方法可能会增加一些运行时开销。

4. **先检查再使用**:
   可以先使用`is_some()`或`is_none()`方法来检查`Option`是否有值，然后再决定如何使用它。这种方法适用于简单的场景，但可能不够优雅。

综合考虑，使用模式匹配的引用是解决这类问题的最直接且高效的方式，它既遵守了Rust的所有权规则，又保持了代码的简洁性。
