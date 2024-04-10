# Exercise 35

- Name: ```enums2```
- Path: ```exercises/enums/enums2.rs```
#### Hint: 

You can create enumerations that have different variants with different types
such as no data, anonymous structs, a single string, tuples, ...etc


---



```rust,editable
// enums2.rs
//
// Execute `rustlings hint enums2` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

#[derive(Debug)]
enum Message {
    // TODO: define the different variants used below
}

impl Message {
    fn call(&self) {
        println!("{:?}", self);
    }
}

fn main() {
    let messages = [
        Message::Move { x: 10, y: 30 },
        Message::Echo(String::from("hello world")),
        Message::ChangeColor(200, 255, 255),
        Message::Quit,
    ];

    for message in &messages {
        message.call();
    }
}

```

---

### 本题内容：

这个练习让你深入探索枚举的另一个强大特性：变体可以有不同的类型和数据结构。你需要定义一个名为`Message`的枚举，它包含四种变体：`Move`、`Echo`、`ChangeColor`和`Quit`，每种变体都有不同的数据关联。

### 相关知识点：

- **枚举变体的数据**：Rust的枚举变体可以关联不同类型的数据，包括没有数据、单一数据、元组以及类似结构体的命名数据。

- **派生特质（Derive Trait）**：通过为枚举派生`Debug`特质，可以在调试时方便地打印枚举变体的信息。

### 解题方法：

#### 定义枚举变体及其数据
- **步骤描述**：
  1. `Move`变体关联一个具名结构体，包含`x`和`y`两个字段，表示移动的位置。
  2. `Echo`变体关联一个`String`类型的数据，表示需要回显的消息。
  3. `ChangeColor`变体关联一个元组，包含三个`i32`类型的数据，表示颜色变更的RGB值。
  4. `Quit`变体不关联任何数据，表示一个退出的信号。

### 代码示例：

```rust
#[derive(Debug)]
enum Message {
    Move { x: i32, y: i32 },
    Echo(String),
    ChangeColor(i32, i32, i32),
    Quit,
}

impl Message {
    fn call(&self) {
        println!("{:?}", self);
    }
}

fn main() {
    let messages = [
        Message::Move { x: 10, y: 30 },
        Message::Echo(String::from("hello world")),
        Message::ChangeColor(200, 255, 255),
        Message::Quit,
    ];

    for message in &messages {
        message.call();
    }
}
```

通过完成这个练习，你将进一步了解枚举的灵活性和强大功能。枚举允许你以类型安全的方式表示一个值可以是几种不同类型中的任意一种，每种类型都可以有不同的数据和结构。这种特性在许多场景中非常有用，例如状态管理、错误处理、消息系统等。掌握枚举和其变体的使用，是成为一名高效Rust开发者的重要一步。

## 扩展知识点与解答：

在完成`enums2`练习之后，你已经掌握了如何在枚举中定义不同类型的变体，以及如何为枚举实现方法。这个练习是对Rust枚举功能的一个深入探索，展示了枚举在构建灵活且强类型的系统中的强大能力。以下是与本题相关的一些扩展知识点及其在Rust编程中的应用。

### 扩展知识点：

1. **枚举的模式匹配**：
   - 与`enums1`练习中简介的内容相同，枚举最强大的一点在于能够与`match`语句结合使用，进行精确的模式匹配。这在处理枚举变体时提供了无与伦比的类型安全性和灵活性。

2. **枚举关联函数与方法**：
   - 枚举不仅可以定义变体，还可以像结构体一样，通过`impl`块定义方法和关联函数。这使得枚举不仅能够存储数据，还能拥有操作这些数据的逻辑。

3. **使用枚举进行错误处理**：
   - `Result`枚举是Rust标准库中用于错误处理的枚举之一，展示了枚举在实际应用中的一个重要用途。通过定义自己的错误枚举，你可以创建一个既富有表达性又易于管理的错误处理系统。

### 扩展解题方法：

1. **深入探索模式匹配**：
   - 在枚举方法中使用模式匹配，根据不同的变体执行不同的逻辑。这可以用于从枚举变体中提取数据，或者根据变体类型改变方法的行为。

2. **定义和使用枚举的关联函数**：
   - 为枚举定义关联函数，例如一个构造函数，可以提高代码的可用性和可读性。

3. **自定义错误枚举**：
   - 定义一个自己的错误枚举，为每种可能的错误情况提供一个变体。然后在函数返回类型中使用`Result<T, YourErrorEnum>`来提供更丰富的错误信息。

#### 示例代码：

定义一个使用模式匹配的枚举方法：

```rust
impl Message {
    fn show_message(&self) {
        match self {
            Message::Move { x, y } => println!("Move to ({}, {})", x, y),
            Message::Echo(msg) => println!("Echo: {}", msg),
            Message::ChangeColor(r, g, b) => println!("Change color to RGB ({}, {}, {})", r, g, b),
            Message::Quit => println!("Quit"),
        }
    }
}
```
