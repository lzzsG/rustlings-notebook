# Exercise 31

- Name: ```structs1```
- Path: ```exercises/structs/structs1.rs```
#### Hint: 

Rust has more than one type of struct. Three actually, all variants are used to package related data together.
There are normal (or classic) structs. These are named collections of related data stored in fields.
Tuple structs are basically just named tuples.
Finally, Unit-like structs. These don't have any fields and are useful for generics.

In this exercise you need to complete and implement one of each kind.
Read more about structs in The Book: [https://doc.rust-lang.org/book/ch05-01-defining-structs.html](https://doc.rust-lang.org/book/ch05-01-defining-structs.html) ([Chinese version](https://rustwiki.org/zh-CN/book/ch05-01-defining-structs.html))


---



```rust,editable
// structs1.rs
//
// Address all the TODOs to make the tests pass!
//
// Execute `rustlings hint structs1` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

struct ColorClassicStruct {
    // TODO: Something goes here
}

struct ColorTupleStruct(/* TODO: Something goes here */);

#[derive(Debug)]
struct UnitLikeStruct;

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn classic_c_structs() {
        // TODO: Instantiate a classic c struct!
        // let green =

        assert_eq!(green.red, 0);
        assert_eq!(green.green, 255);
        assert_eq!(green.blue, 0);
    }

    #[test]
    fn tuple_structs() {
        // TODO: Instantiate a tuple struct!
        // let green =

        assert_eq!(green.0, 0);
        assert_eq!(green.1, 255);
        assert_eq!(green.2, 0);
    }

    #[test]
    fn unit_structs() {
        // TODO: Instantiate a unit-like struct!
        // let unit_like_struct =
        let message = format!("{:?}s are fun!", unit_like_struct);

        assert_eq!(message, "UnitLikeStructs are fun!");
    }
}

```

---

### 本题内容：

本练习要求你完成并实现三种不同类型的结构体：普通结构体（classic structs）、元组结构体（tuple structs）、以及类单元结构体（unit-like structs），通过填写缺失的部分来使得测试通过。

### 相关知识点：

1. **普通结构体**：它是一种将相关数据打包在一起的命名集合。每个字段都有一个名称，可以用来直接访问结构体中的数据。

2. **元组结构体**：基本上就是命名的元组。它们将数据打包在一起，但不给每个字段命名，而是通过索引访问。

3. **类单元结构体**：没有任何字段的结构体，用于场景，比如需要在泛型中表示某种类型，但不需要实际存储数据。

### 解题方法：

#### 普通结构体
- **步骤描述**：为`ColorClassicStruct`添加三个字段`red`、`green`和`blue`，它们的类型都是`u8`，用于表示颜色值。

#### 元组结构体
- **步骤描述**：定义`ColorTupleStruct`为一个包含三个`u8`类型元素的元组结构体，以表示颜色值。

#### 类单元结构体
- **步骤描述**：`UnitLikeStruct`已经被正确定义为一个类单元结构体，你只需实例化它即可。

### 代码示例：

```rust
// 定义普通结构体
struct ColorClassicStruct {
    red: u8,
    green: u8,
    blue: u8,
}

// 定义元组结构体
struct ColorTupleStruct(u8, u8, u8);

// 类单元结构体已经正确定义

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn classic_c_structs() {
        // 实例化普通结构体
        let green = ColorClassicStruct { red: 0, green: 255, blue: 0 };

        assert_eq!(green.red, 0);
        assert_eq!(green.green, 255);
        assert_eq!(green.blue, 0);
    }

    #[test]
    fn tuple_structs() {
        // 实例化元组结构体
        let green = ColorTupleStruct(0, 255, 0);

        assert_eq!(green.0, 0);
        assert_eq!(green.1, 255);
        assert_eq!(green.2, 0);
    }

    #[test]
    fn unit_structs() {
        // 实例化类单元结构体
        let unit_like_struct = UnitLikeStruct;
        let message = format!("{:?}s are fun!", unit_like_struct);

        assert_eq!(message, "UnitLikeStructs are fun!");
    }
}
```

通过这个练习，你将学会如何定义和使用Rust中的不同类型的结构体。这是理解Rust数据组织方式的重要一步，也是构建复杂数据模型的基础。

## 扩展知识点与解答：

解决`structs1`练习不仅让你熟悉了Rust中结构体的基本概念，还提供了一个机会来探索结构体更深层次的应用和扩展知识点。以下是与本题相关的一些扩展知识点及其在Rust编程中的应用。

### 扩展知识点：

1. **结构体方法和关联函数**：
   - 在Rust中，你可以为结构体定义方法和关联函数。方法是在结构体的实例上调用的，它们第一个参数是`self`，表示方法调用的结构体实例。关联函数是在结构体类型上调用的，它们不接收`self`参数。这些函数定义在`impl`块中。

2. **泛型结构体**：
   - Rust允许你定义泛型结构体，这意味着结构体的字段类型可以是泛型的。这提供了极大的灵活性，允许结构体被用于多种类型的数据。

3. **结构体的所有权和借用**：
   - 当结构体包含引用时，你需要考虑生命周期问题，确保结构体不会比它所包含的引用活得更久。这通常通过生命周期注解来解决。

4. **派生特质（Derive Trait）**：
   - Rust允许自动为结构体实现一些特质，例如`Debug`、`Clone`等。这是通过在结构体定义前使用`#[derive]`属性来完成的。

### 扩展解题方法：

- **实现结构体方法**：
  - 为`ColorClassicStruct`和`ColorTupleStruct`实现一个方法，比如`new`关联函数来创建结构体实例，或者`to_hex`方法来返回颜色的十六进制表示。

- **定义泛型结构体**：
  - 尝试将`ColorClassicStruct`或`ColorTupleStruct`改写为泛型结构体，使其可以支持不同类型的颜色表示，例如，除了`u8`之外，还可以用`f32`类型来表示颜色的比例值。

- **探索生命周期注解**：
  - 如果你的结构体包含引用，尝试添加生命周期注解以确保代码的正确性和安全性。

- **自动派生特质**：
  - 给你的结构体添加`#[derive(Debug, Clone, PartialEq)]`，这样就可以打印结构体实例、克隆它们，或比较两个实例了。

### 实践建议：

- **尽可能使用结构体方法**，因为它们提供了良好的封装性和模块化，让代码更清晰、易于维护。

- **在合适的场合使用泛型**，泛型可以让你的代码更加灵活和复用。

- **在结构体包含引用时仔细考虑生命周期**，正确的生命周期注解可以保证代码的安全性。
