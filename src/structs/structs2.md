# Exercise 32

- Name: ```structs2```
- Path: ```exercises/structs/structs2.rs```
#### Hint: 

Creating instances of structs is easy, all you need to do is assign some values to its fields.
There are however some shortcuts that can be taken when instantiating structs.

Have a look in The Book, to find out more: [https://doc.rust-lang.org/stable/book/ch05-01-defining-structs](https://doc.rust-lang.org/stable/book/ch05-01-defining-structs.html#creating-instances-from-other-instances-with-struct-update-syntax) ([Chinese version](https://rustwiki.org/zh-CN/book/ch05-01-defining-structs.html#%E4%BD%BF%E7%94%A8%E7%BB%93%E6%9E%84%E4%BD%93%E6%9B%B4%E6%96%B0%E8%AF%AD%E6%B3%95%E4%BB%8E%E5%85%B6%E4%BB%96%E5%AE%9E%E4%BE%8B%E5%88%9B%E5%BB%BA%E5%AE%9E%E4%BE%8B))


---



```rust,editable
// structs2.rs
//
// Address all the TODOs to make the tests pass!
//
// Execute `rustlings hint structs2` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

#[derive(Debug)]
struct Order {
    name: String,
    year: u32,
    made_by_phone: bool,
    made_by_mobile: bool,
    made_by_email: bool,
    item_number: u32,
    count: u32,
}

fn create_order_template() -> Order {
    Order {
        name: String::from("Bob"),
        year: 2019,
        made_by_phone: false,
        made_by_mobile: false,
        made_by_email: true,
        item_number: 123,
        count: 0,
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn your_order() {
        let order_template = create_order_template();
        // TODO: Create your own order using the update syntax and template above!
        // let your_order =
        assert_eq!(your_order.name, "Hacker in Rust");
        assert_eq!(your_order.year, order_template.year);
        assert_eq!(your_order.made_by_phone, order_template.made_by_phone);
        assert_eq!(your_order.made_by_mobile, order_template.made_by_mobile);
        assert_eq!(your_order.made_by_email, order_template.made_by_email);
        assert_eq!(your_order.item_number, order_template.item_number);
        assert_eq!(your_order.count, 1);
    }
}

```

---

### 本题内容：

这个练习要求你利用结构体更新语法来创建一个`Order`结构体的新实例。这个语法允许你基于另一个结构体实例来快速创建新实例，同时对一些字段进行修改。

### 相关知识点：

- **结构体更新语法**：结构体更新语法允许你使用已有的结构体实例来创建新的实例。对于那些你没有指定值的字段，Rust会自动使用原实例中相应字段的值。

- **结构体实例的创建**：创建结构体实例时，需要为所有字段指定值，除非使用结构体更新语法。

### 解题方法：

#### 使用结构体更新语法创建实例
- **步骤描述**：
  1. 使用`create_order_template`函数创建一个`Order`的模板实例。
  2. 基于这个模板实例，使用结构体更新语法来创建一个新的`Order`实例。在这个过程中，只修改`name`和`count`字段的值，其余字段保留模板实例中的值。

#### 代码示例：

```rust
#[derive(Debug)]
struct Order {
    name: String,
    year: u32,
    made_by_phone: bool,
    made_by_mobile: bool,
    made_by_email: bool,
    item_number: u32,
    count: u32,
}

fn create_order_template() -> Order {
    Order {
        name: String::from("Bob"),
        year: 2019,
        made_by_phone: false,
        made_by_mobile: false,
        made_by_email: true,
        item_number: 123,
        count: 0,
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn your_order() {
        let order_template = create_order_template();
        // 使用结构体更新语法创建新的Order实例
        let your_order = Order {
            name: String::from("Hacker in Rust"),
            count: 1,
            ..order_template // 使用..语法引用order_template中未明确指定的其他字段值
        };
        assert_eq!(your_order.name, "Hacker in Rust");
        assert_eq!(your_order.year, order_template.year);
        assert_eq!(your_order.made_by_phone, order_template.made_by_phone);
        assert_eq!(your_order.made_by_mobile, order_template.made_by_mobile);
        assert_eq!(your_order.made_by_email, order_template.made_by_email);
        assert_eq!(your_order.item_number, order_template.item_number);
        assert_eq!(your_order.count, 1);
    }
}
```

通过使用结构体更新语法，你可以更加灵活和高效地创建结构体实例，特别是当你需要基于一个现有的实例来创建一个稍微不同的新实例时。这种方式在处理具有许多字段的结构体时尤其有用，因为它避免了对共享字段的重复指定。掌握这种语法是深入理解Rust语言特性和提高编码效率的重要一步。

## 扩展知识点与解答：

解决`structs2`练习引入了结构体更新语法，这是Rust中创建结构体实例时的一种便捷方式，尤其当你想基于某个已有实例创建新实例，同时只更改其中几个字段的值时。这个练习不仅加深了对基本结构体使用的理解，还为深入学习Rust提供了良好的起点。下面探讨与此练习相关的一些扩展知识点及解题方法。

### 扩展知识点：

1. **可变性与结构体**：
   - 结构体字段的可变性是由其所属实例的可变性决定的。即使结构体中的字段本身没有被标记为`mut`，只要结构体实例是可变的，这些字段也可以被修改。

2. **字段初始化简写**：
   - 当结构体的字段名与变量名相同时，你可以使用字段初始化简写语法来初始化结构体。这使得代码更加简洁。

3. **派生特质（Derive Trait）**：
   - 使用`#[derive]`属性，可以为结构体自动实现特定的特质，如`Debug`、`Clone`、`Copy`、`PartialEq`等，这些特质使得结构体更加易用。

4. **模式匹配与结构体**：
   - Rust的模式匹配可以与结构体一起使用，这在需要根据结构体的不同字段值执行不同操作时非常有用。

### 扩展解题方法：

- **实现结构体方法**：
  - 通过在`impl`块中定义方法，可以为结构体增加行为。例如，可以为`Order`结构体实现一个`new`函数，用于创建新的`Order`实例，或者实现其他自定义方法处理订单数据。

- **使用`Clone`特质**：
  - 如果你需要基于某个实例创建一个完全独立的新实例，可以使用`clone`方法，前提是该结构体实现了`Clone`特质。这在需要复制复杂结构体时特别有用。

- **模式匹配结构体**：
  - 使用模式匹配来解构结构体，可以基于结构体的不同字段值执行不同的逻辑。这是处理基于复杂条件的逻辑分支的强大工具。

#### 实践示例：

假设你想为`Order`结构体实现一个方法，检查订单是否通过手机制作：

```rust
impl Order {
    // 检查订单是否通过手机制作
    fn is_made_by_mobile(&self) -> bool {
        self.made_by_mobile
    }
}
```

### 关于\#[derive(...)]

在Rust中，`#[derive(...)]`属性是一个非常实用的功能，它允许你自动为类型实现某些特质（traits）。这些特质可以是Rust标准库提供的，如`Debug`、`Copy`、`Clone`等，也可以是第三方库定义的特质，前提是它们支持自动派生。

当你为一个结构体或枚举添加`#[derive(...)]`属性时，Rust编译器会自动生成特质实现的代码，这大大减少了样板代码的编写，使得开发更加高效。

以下是一些常见特质的简要说明：

- **Debug**: 实现了`fmt::Debug`特质的类型可以使用`{:?}`标记符通过`println!`宏打印，这对于快速调试是非常有用的。
- **PartialEq**: 这个特质允许你比较类型的实例是否相等。`PartialEq`是"部分相等"的缩写，意味着实现时可能不是所有的值都可以比较。
- **Eq**: `Eq`是一个标记特质（没有方法），它表明你的类型是完全相等的，也就是说，它扩展了`PartialEq`，提供了一个所有值都可以相互比较的保证。
- **PartialOrd** 和 **Ord**: 这些特质允许类型的实例进行排序。`PartialOrd`允许部分排序，而`Ord`则要求类型的所有实例都可以被完全排序。
- **Copy** 和 **Clone**: `Copy`特质表示类型的值可以通过简单的位复制来复制，而不需要特别的克隆方法。它通常用于简单的值类型（如整数类型）。`Clone`特质允许类型的实例被显式复制，这在处理所有权时非常有用，尤其是当类型的复制操作涉及到比简单位复制更复杂的逻辑时。

例如，你有一个结构体`Point`，你希望能够打印它的实例（调试目的）、比较两个实例是否相等、克隆实例。你可以这样做：

```rust
#[derive(Debug, PartialEq, Clone)]
struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let point1 = Point { x: 1, y: 2 };
    let point2 = point1.clone(); // 使用了Clone特质
    println!("{:?}", point1); // 使用了Debug特质
    assert_eq!(point1, point2); // 使用了PartialEq特质
}
```

通过简单地使用`#[derive(...)]`，我们避免了为`Point`类型手动实现`fmt::Debug`、`PartialEq`和`Clone`特质的需求。这展示了Rust`#[derive]`属性的强大和便利性。
