# Exercise 57

- Name: ```generics1```
- Path: ```exercises/generics/generics1.rs```
#### Hint: 

Vectors in Rust make use of generics to create dynamically sized arrays of any type.
You need to tell the compiler what type we are pushing onto this vector.


---



```rust,editable
// generics1.rs
//
// This shopping list program isn't compiling! Use your knowledge of generics to
// fix it.
//
// Execute `rustlings hint generics1` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

fn main() {
    let mut shopping_list: Vec<?> = Vec::new();
    shopping_list.push("milk");
}

```

---

### 本题内容

本练习的目的是引导学生理解和使用 Rust 中的泛型（Generics），特别是如何在使用 `Vec`（向量）时指定元素类型。通过修复给出的代码，学生将学习如何声明一个具有特定类型元素的向量。

### 相关知识点

- **泛型**：泛型是允许代码对不同的数据类型进行操作的特性。在 Rust 中，泛型广泛应用于各种数据结构和函数中，使它们能够更加灵活和重用。
- **向量（`Vec`）**：向量是 Rust 中一个允许存储多个值的动态数组。向量是泛型的，它可以包含任意类型的元素，但同一个向量中的所有元素必须是相同的类型。

### 解题方法

要修复这个编译错误，你需要在声明 `shopping_list` 变量时指定向量的元素类型。因为我们要在向量中存储字符串，所以应该使用字符串类型 `&str` 作为元素类型。

1. **指定向量元素的类型**：在声明向量时，通过在 `Vec` 后加上 `<T>` 来指定元素类型，其中 `T` 是元素的类型。在这个例子中，`T` 应该是 `&str`，因为我们要存储字符串字面值。

### 代码示例

根据上述解题方法，以下是修改后的代码：

```rust
fn main() {
    // 指定向量的元素类型为 &str
    let mut shopping_list: Vec<&str> = Vec::new();
    shopping_list.push("milk");
}
```

在这个修改中，我们通过在 `Vec::new()` 后添加 `<&str>` 来明确指定了 `shopping_list` 向量应该存储字符串切片（`&str`）类型的元素。这样，编译器就知道我们打算在这个向量中存储什么类型的值了。

这个简单的练习展示了泛型在 Rust 中的基本应用，尤其是在使用标准库中的泛型集合（如 `Vec`）时，如何通过指定具体的类型参数来使用它们。理解并掌握泛型的使用对于编写高效、可重用的 Rust 代码至关重要。

## 扩展知识点与解答：

### 扩展知识点

泛型是 Rust 编程中不可或缺的一部分，不仅用于容器类型如 `Vec<T>`，还广泛应用于函数、结构体、枚举、方法等。掌握泛型可以提高代码的灵活性和复用性。以下是一些扩展知识点：

1. **泛型函数与方法**：可以定义接受一种或多种泛型类型参数的函数和方法，这使得它们能够操作不同类型的数据。
2. **泛型结构体与枚举**：结构体和枚举也可以是泛型的，这允许你在定义数据结构时使用泛型类型。
3. **trait bounds**：通过为泛型类型指定 trait bounds（特征界限），可以约束泛型类型必须实现特定的行为。这是通过实现特定的 trait 来实现的。
4. **生命周期参数**：泛型还包括生命周期参数，它们用于指定引用的有效范围，以确保数据的安全使用。
5. **类型推导**：在很多情况下，Rust 能够根据上下文自动推导泛型类型参数，使得代码更加简洁。

### 扩展解题方法

掌握泛型的使用不仅限于解决当前的问题，还可以扩展到更复杂的场景。以下是一些扩展的解题方法：

1. **泛型数据结构**：尝试定义一个泛型结构体，该结构体可以存储不同类型的数据。例如，创建一个泛型 `Point<T>` 结构体，它可以用来表示二维空间中的点，无论是整数还是浮点数。

   ```rust
   struct Point<T> {
       x: T,
       y: T,
   }
   ```

2. **泛型函数**：编写一个接受泛型参数的函数，例如一个返回两个参数中较大值的函数，这要求泛型类型实现了 `PartialOrd` 和 `Copy` trait。

   ```rust
   fn max<T: PartialOrd + Copy>(a: T, b: T) -> T {
       if a > b { a } else { b }
   }
   ```

3. **使用 trait bounds**：当你需要对泛型类型执行某些操作时，可能需要它们实现特定的 trait。实践使用 trait bounds 来约束泛型类型必须拥有的行为。

4. **探索标准库中的泛型**：Rust 标准库中有许多使用泛型的例子，如 `Option<T>`、`Result<T, E>` 等。通过阅读和理解这些代码，可以深入了解泛型的实际应用。
