# strings

---

在开始Rust的字符串练习之前，我们将探索Rust中字符串处理的一些基础和进阶概念。通过这些练习，你将学会如何在Rust中有效地使用和操作字符串，理解`String`与`&str`之间的区别，以及如何在实际编程中应用它们。

#### 字符串基础

Rust中的字符串处理主要涉及两种类型：`String`和`&str`。`String`是一个可增长的、堆分配的字符串类型，支持对字符串内容进行修改；而`&str`则是一个字符串切片，通常用于借用另一个字符串的部分或全部，它是不可变的。

在这一系列练习中，你将遇到如下挑战：

- **如何将字符串切片转换为`String`**：你会学到使用`.to_string()`方法或`String::from()`函数来完成这一转换。
- **引用转换**：通过添加一个字符来改变引用类型，深入了解如何将`String`隐式转换为`&str`。
- **字符串的有效操作**：掌握去除字符串首尾空白、字符串拼接以及替换字符串中的特定子串等常用操作。

#### 高级字符串处理

通过这些练习，你还将深入学习Rust中的高级字符串处理技巧，包括：

- **字符串切片与索引操作**：虽然直接通过索引访问字符串中的字符在Rust中并不总是允许的（因为Rust字符串是UTF-8编码的，单个字符可能占用多个字节），但你可以通过字符串切片来安全地访问字符串的部分内容。
- **字符串迭代**：Rust允许你迭代字符串中的每个字符或字节，这对于字符编码敏感的操作非常有用。

#### 字符串练习概览

- **`strings1`练习**将引导你理解如何从字符串切片创建一个`String`对象。
- **`strings2`练习**将挑战你使用简单的字符添加，将`String`类型隐式转换为`&str`类型。
- **`strings3`练习**聚焦于使用Rust标准库中的字符串方法来执行常见的字符串操作，如去除空白、字符串拼接和内容替换。
- **`strings4`练习**提供了一系列的字符串值，要求你根据它们是`String`类型还是`&str`类型来调用正确的函数。

在完成这些练习后，你将对Rust中的字符串处理有更深入的了解，这对于编写高效且安全的Rust代码至关重要。记住，虽然挑战看似简单，但它们包含了Rust编程中的核心概念，理解这些基础知识对于你未来的Rust编程之旅至关重要。

---

```rust
// 你不必现在理解以下代码，不过你可以尝试运行它。

fn main() {
    let mut explorer = Explorer::named("费里斯");

    explorer.discover("古树下埋藏着的密信".into());
    if explorer.has("魔法罗盘") {
        println!("{} 使用魔法罗盘解读了密信。", explorer.name);
        explorer.advance("一个隐藏的路径出现了。");
    }

    let puzzle = "石碑上的谜语：夜晚来临，我却消失。我是什么？";
    match explorer.solve_puzzle(puzzle) {
        Some(answer) => println!("{} 解开了谜语：{}", explorer.name, answer),
        None => println!("谜语仍未解开。"),
    }

    println!("\n旅程记录：\n{}\n\n你的旅程继续延伸进入这片暗黑森林。🍃🌲", explorer.journey_summary());
}

struct Explorer {
    name: String,
    belongings: Vec<String>,
    discoveries: Vec<String>,
}

impl Explorer {
    fn named(name: &str) -> Self {
        Self {
            name: name.to_string(),
            belongings: vec!["魔法罗盘".to_string()],
            discoveries: Vec::new(),
        }
    }

    fn discover(&mut self, discovery: String) {
        println!("{} 发现了：{}", self.name, discovery);
        self.discoveries.push(discovery);
    }

    fn has(&self, item: &str) -> bool {
        self.belongings.contains(&item.to_string())
    }

    fn solve_puzzle(&self, puzzle: &str) -> Option<String> {
        println!("{} 遇到了谜题：{}", self.name, puzzle);
        Some("影子".to_string())
    }

    fn advance(&mut self, event: &str) {
        println!("{} 前进了：{}", self.name, event);
        self.discoveries.push(event.to_string());
    }

    fn journey_summary(&self) -> String {
        self.discoveries.join("\n")
    }
}
```
