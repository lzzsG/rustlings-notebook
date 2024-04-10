# Exercise 36

- Name: ```enums3```
- Path: ```exercises/enums/enums3.rs```
#### Hint: 

As a first step, you can define enums to compile this code without errors.
and then create a match expression in `process()`.
Note that you need to deconstruct some message variants
in the match expression to get value in the variant.


---



```rust,editable
// enums3.rs
//
// Address all the TODOs to make the tests pass!
//
// Execute `rustlings hint enums3` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

enum Message {
    // TODO: implement the message variant types based on their usage below
}

struct Point {
    x: u8,
    y: u8,
}

struct State {
    color: (u8, u8, u8),
    position: Point,
    quit: bool,
    message: String
}

impl State {
    fn change_color(&mut self, color: (u8, u8, u8)) {
        self.color = color;
    }

    fn quit(&mut self) {
        self.quit = true;
    }

    fn echo(&mut self, s: String) { self.message = s }

    fn move_position(&mut self, p: Point) {
        self.position = p;
    }

    fn process(&mut self, message: Message) {
        // TODO: create a match expression to process the different message
        // variants
        // Remember: When passing a tuple as a function argument, you'll need
        // extra parentheses: fn function((t, u, p, l, e))
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_match_message_call() {
        let mut state = State {
            quit: false,
            position: Point { x: 0, y: 0 },
            color: (0, 0, 0),
            message: "hello world".to_string(),
        };
        state.process(Message::ChangeColor(255, 0, 255));
        state.process(Message::Echo(String::from("hello world")));
        state.process(Message::Move(Point { x: 10, y: 15 }));
        state.process(Message::Quit);

        assert_eq!(state.color, (255, 0, 255));
        assert_eq!(state.position.x, 10);
        assert_eq!(state.position.y, 15);
        assert_eq!(state.quit, true);
        assert_eq!(state.message, "hello world");
    }
}

```

---

### 本题内容：

本练习要求你深入枚举的使用，通过定义一个`Message`枚举和在`State`结构体的`process`方法中创建一个匹配表达式来处理不同的消息类型。`Message`枚举将包括`ChangeColor`、`Echo`、`Move`和`Quit`这四种变体，每种变体都会携带不同的数据。

### 相关知识点：

- **枚举变体的数据携带**：枚举变体可以关联数据，这些数据可以是任何类型，包括但不限于原始数据类型、字符串、元组、甚至是其他结构体。

- **模式匹配与枚举**：`match`表达式是Rust中处理枚举的强大工具，允许你基于枚举变体执行不同的代码路径。通过模式匹配，你可以轻松地提取枚举变体中的数据。

- **模式解构**：在模式匹配中，你可以解构枚举变体来获取它们携带的数据，这对于执行变体特定的操作非常有用。

### 解题方法：

#### 定义`Message`枚举及其变体
- **步骤描述**：
  1. 为`Message`枚举定义四种变体：`ChangeColor`携带一个RGB颜色元组；`Echo`携带一个字符串；`Move`携带一个`Point`结构体实例；`Quit`不携带数据。

#### 实现`process`方法中的匹配表达式
- **步骤描述**：
  1. 使用`match`表达式来处理传入的`Message`。
  2. 对于每种消息类型，调用`State`结构体对应的方法来更新状态。

### 代码示例：

```rust
enum Message {
    ChangeColor(u8, u8, u8),
    Echo(String),
    Move(Point),
    Quit,
}

struct Point {
    x: u8,
    y: u8,
}

struct State {
    color: (u8, u8, u8),
    position: Point,
    quit: bool,
    message: String,
}

impl State {
    // 省略了已提供的方法...

    fn process(&mut self, message: Message) {
        match message {
            Message::ChangeColor(r, g, b) => self.change_color((r, g, b)),
            Message::Echo(s) => self.echo(s),
            Message::Move(p) => self.move_position(p),
            Message::Quit => self.quit(),
        }
    }
}

// 测试代码省略...
```

通过完成这个练习，你将进一步掌握如何在Rust中灵活使用枚举和模式匹配来处理不同的情况。这种模式在构建需要处理多种可能输入或事件的应用程序时尤为重要，例如UI事件处理、消息队列处理等。枚举和模式匹配的结合使用是Rust强大表达能力的明显例证，能够帮助你编写既简洁又易于维护的代码。

## 扩展知识点与解答：

在完成`enums3`练习之后，你应该对Rust中枚举的高级使用有了更深的理解。这个练习展示了枚举如何有效地用于表示多种不同类型的数据，并且通过模式匹配的方式来处理这些数据。以下是一些与本题相关的扩展知识点和解题方法，它们可以帮助你在实际项目中更加灵活地使用枚举。

### 扩展知识点：

1. **枚举与错误处理**：
   - Rust的`Result`枚举是错误处理的核心，通过`Ok`和`Err`变体可以表示操作的成功或失败。自定义错误类型通常通过枚举来实现，利用枚举变体表示不同的错误情况。

2. **枚举和泛型**：
   - 枚举可以和泛型结合使用，允许在枚举变体中存储泛型数据。这提高了枚举的灵活性，使其能够用于更广泛的场景。

3. **枚举的模式匹配**：
   - 通过`match`或`if let`来匹配枚举变体时，可以从中解构出数据。这对于根据枚举变体执行特定逻辑非常有用。

### 扩展解题方法：

1. **枚举变体的关联函数**：
   - 除了可以为枚举实现方法外，还可以定义关联函数，作为构造器使用。例如，可以定义一个`new`函数来创建枚举的实例。

2. **利用枚举处理多态**：
   - 通过为枚举实现特质（trait），可以利用枚举来实现多态。这意味着你可以对枚举实现的特质方法进行调用，而实际上这些调用会根据枚举的具体变体来执行不同的逻辑。

3. **枚举的递归变体**：
   - Rust允许枚举变体包含枚举本身，这种递归枚举可以用来表示如树这样的复杂数据结构。使用递归枚举时，通常需要借助`Box`来创建指针，因为Rust需要知道数据的大小。

#### 示例代码：

定义枚举的关联函数和模式匹配：

```rust
enum Message {
    Move { x: i32, y: i32 },
    Write(String),
    // 其他变体...
}

impl Message {
    fn new_move(x: i32, y: i32) -> Self {
        Message::Move { x, y }
    }
    
    fn call(&self) {
        match self {
            Message::Move { x, y } => println!("Move to ({}, {})", x, y),
            Message::Write(text) => println!("Text message: {}", text),
            // 处理其他变体...
        }
    }
}
```
