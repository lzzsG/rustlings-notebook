# Exercise 58

- Name: ```generics2```
- Path: ```exercises/generics/generics2.rs```
#### Hint: 

Currently we are wrapping only values of type 'u32'.
Maybe we could update the explicit references to this data type somehow?

If you are still stuck [https://doc.rust-lang.org/stable/book/ch10-01-syntax.html#in-method-definitions](https://doc.rust-lang.org/stable/book/ch10-01-syntax.html#in-method-definitions) ([Chinese version](https://rustwiki.org/zh-CN/book/ch10-01-syntax.html#%E6%96%B9%E6%B3%95%E5%AE%9A%E4%B9%89%E4%B8%AD%E7%9A%84%E6%B3%9B%E5%9E%8B))



---



```rust,editable
// generics2.rs
//
// This powerful wrapper provides the ability to store a positive integer value.
// Rewrite it using generics so that it supports wrapping ANY type.
//
// Execute `rustlings hint generics2` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

struct Wrapper {
    value: u32,
}

impl Wrapper {
    pub fn new(value: u32) -> Self {
        Wrapper { value }
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn store_u32_in_wrapper() {
        assert_eq!(Wrapper::new(42).value, 42);
    }

    #[test]
    fn store_str_in_wrapper() {
        assert_eq!(Wrapper::new("Foo").value, "Foo");
    }
}

```

---

### 本题内容

本题目的是引导学生学习和实践如何使用 Rust 的泛型来创建可以存储任意类型值的结构体。通过修改给出的 `Wrapper` 结构体，使其不再限制于只能存储 `u32` 类型的值，而是可以支持存储任意类型的值。

### 相关知识点

- **泛型**：允许在定义函数、结构体、枚举、或方法时使用泛型类型。使用泛型可以提高代码的复用性和灵活性。
- **泛型结构体**：定义时使用一种或多种泛型类型的结构体。泛型类型通常在结构体名后的尖括号内指定。
- **泛型方法**：结构体的方法也可以是泛型的，允许对结构体中的泛型数据执行操作。

### 解题方法

要完成这个练习，需要对 `Wrapper` 结构体和它的 `new` 方法应用泛型编程。具体步骤如下：

1. **修改 `Wrapper` 结构体**：在结构体定义时引入泛型类型 `T`，并将 `value` 字段的类型从 `u32` 改为 `T`。
2. **修改 `new` 方法**：将 `new` 方法的参数类型从 `u32` 改为泛型类型 `T`，并确保这个类型与结构体定义中的泛型类型相匹配。

### 代码示例

根据上述解题方法，以下是修改后的代码：

```rust
// 使用泛型参数T改写Wrapper结构体
struct Wrapper<T> {
    value: T,
}

impl<T> Wrapper<T> {
    // new方法也使用泛型参数T
    pub fn new(value: T) -> Self {
        Wrapper { value }
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn store_u32_in_wrapper() {
        assert_eq!(Wrapper::new(42).value, 42);
    }

    #[test]
    fn store_str_in_wrapper() {
        assert_eq!(Wrapper::new("Foo").value, "Foo");
    }
}
```

在这个修改中：

- `Wrapper` 结构体现在可以接受一个泛型类型 `T`，这意味着它可以用来存储任意类型的值。
- `new` 方法也被修改为接受一个类型为 `T` 的参数，使得我们可以创建包含不同类型值的 `Wrapper` 实例。
- 测试用例展示了如何使用修改后的 `Wrapper` 结构体来存储 `u32` 类型和 `&str` 类型的值。

这个练习展示了泛型在提高 Rust 代码复用性和灵活性方面的强大功能。通过使用泛型，可以创建出更加通用和灵活的数据结构和函数。

## 扩展知识点与解答：

### 扩展知识点

1. **泛型约束（Bounds）**：在使用泛型时，我们可以对泛型参数施加一些约束，这些约束称为泛型约束。通过约束，我们可以限制泛型类型必须实现某些特质（Trait），这允许我们在泛型函数或结构体中使用这些特质定义的方法。

2. **生命周期与泛型**：在处理引用时，Rust 需要确保数据的有效性，这就引入了生命周期的概念。泛型和生命周期经常一起使用，特别是当泛型类型是某种引用时。理解如何将生命周期参数与泛型结合使用是深入学习 Rust 必不可少的一部分。

3. **特质对象与泛型**：特质对象（Trait Objects）提供了一种动态分发特质方法的方式，而泛型则是静态分发。在某些情况下，使用特质对象而不是泛型可以提供更大的灵活性，尤其是在处理多态数据时。

4. **高级类型系统特性**：Rust 的类型系统提供了一些高级特性，如关联类型、默认泛型参数等，这些特性在设计复杂的泛型数据结构和函数时非常有用。

### 扩展解题方法

1. **实现泛型约束**：尝试给你的泛型结构体或函数添加一些约束条件。例如，你可以要求泛型类型 `T` 必须实现 `Display` 特质，这样就可以在函数内部打印 `T` 类型的值。

    ```rust
    use std::fmt::Display;

    struct Wrapper<T: Display> {
        value: T,
    }

    impl<T: Display> Wrapper<T> {
        pub fn new(value: T) -> Self {
            println!("Creating a new Wrapper for a value: {}", value);
            Wrapper { value }
        }
    }
    ```

2. **结合生命周期和泛型**：如果你的泛型类型中包含了引用，考虑如何添加生命周期参数来确保你的代码安全。

    ```rust
    struct Wrapper<'a, T: 'a> {
        value: &'a T,
    }

    impl<'a, T> Wrapper<'a, T> {
        pub fn new(value: &'a T) -> Self {
            Wrapper { value }
        }
    }
    ```

3. **探索特质对象与泛型的使用场景**：在某些需要处理多态数据的场合，比较特质对象和泛型的优劣，理解它们在实际应用中的权衡。

4. **利用高级类型系统特性**：尝试使用关联类型、默认泛型参数等 Rust 的高级类型系统特性，来构建更复杂的泛型数据结构和函数。

通过掌握这些扩展知识点和解题方法，你将能够更加灵活地使用泛型来构建复杂且高效的 Rust 应用。泛型是 Rust 强大类型系统的核心部分，深入理解泛型的高级用法对于成为一名高级 Rust 开发者至关重要。
