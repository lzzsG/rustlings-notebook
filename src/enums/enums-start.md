# enums

---

在接下来的一系列`enums`练习中，我们将深入探索Rust中枚举的使用。枚举是Rust的核心特性之一，提供了一种方法来定义一个类型，该类型可以有固定数量的变体，每个变体可以携带不同类型和数量的数据。通过这些练习，你将学习如何定义和使用枚举，以及如何通过模式匹配来处理枚举变体。

从`enums1`开始，你将遇到一个需要定义几种消息类型的简单任务。这个练习将帮助你掌握基本的枚举定义方法，并理解如何使用枚举来表示多种可能的值。

接下来的`enums2`练习进一步扩展了这个概念，要求你创建一个更复杂的枚举，其中的变体可以包含不同类型的数据，比如无数据、匿名结构体、字符串和元组等。这个练习将展示枚举在存储和处理多种数据类型时的灵活性。

最后，`enums3`练习要求你利用枚举和模式匹配来执行更复杂的数据处理。通过定义一个`Message`枚举和一个`State`结构体，你将学习如何根据不同的消息类型更新应用程序的状态。这个练习强调了枚举在构建状态机和处理事件系统中的重要性。

在完成这些练习后，你将对Rust中枚举的使用有一个全面的了解，包括它们如何定义、如何携带数据，以及如何通过模式匹配来处理它们。枚举是构建Rust应用程序的基石之一，无论是用于错误处理、消息传递还是状态管理，掌握枚举都是提升你的Rust编程技能的关键步骤。

这些练习只是Rust枚举使用方法的起点，Rust提供的枚举功能非常强大，它们可以用于创建类型安全的代码，减少运行时错误。希望你能享受这一系列练习带来的挑战和学习机会。

---

```rust
// 你不必现在理解以下代码，不过你可以尝试运行它。

enum ForestMessage {
    Whisper(String),
    WindGust(u32),
    Rustle { leaves: u32, direction: String },
    Silence,
}

struct Explorer {
    name: String,
    stamina: i32,
}

impl Explorer {
    fn new(name: String) -> Self {
        Self { name, stamina: 100 }
    }

    fn interpret_message(&mut self, message: ForestMessage) {
        match message {
            ForestMessage::Whisper(text) => {
                println!("{} hears a whisper: {}", self.name, text);
                self.stamina += 5;
            }
            ForestMessage::WindGust(strength) => {
                println!("{} braces against a gust of wind with strength {}!", self.name, strength);
                self.stamina -= strength as i32;
            }
            ForestMessage::Rustle { leaves, direction } => {
                println!("{} notices the rustle of {} leaves coming from the {}.", self.name, leaves, direction);
                self.stamina -= 10;
            }
            ForestMessage::Silence => {
                println!("An unnerving silence falls over the forest around {}.", self.name);
                self.stamina += 10;
            }
        }
    }
}

fn main() {
    let mut explorer = Explorer::new("Ferris the Fearless".to_string());
    
    let messages = vec![
        ForestMessage::Whisper("Beware the fallen log...".to_string()),
        ForestMessage::WindGust(20),
        ForestMessage::Rustle { leaves: 100, direction: "north".to_string() },
        ForestMessage::Silence,
    ];

    for message in messages {
        explorer.interpret_message(message);
    }

    println!("\nWith stamina at {}\n{}'s journey continues into the dark forest.🍃", explorer.stamina, explorer.name);
}
```
