# options

---

在接下来的一系列 `Option` 练习中，你将会深入探索 Rust 中的 `Option` 枚举，这是 Rust 用于处理可选值和空值（null）的一种安全、强大的机制。通过这些练习，你将学习如何使用 `Option` 来表示可能存在或不存在的值，以及如何安全地处理这些值，从而避免在编程中常见的空值引用错误。

### 练习 48 (`options1`)

在这个练习中，你需要实现一个函数 `maybe_clean_water`，该函数根据一天中的时间来判断冰淇淋的剩余量。这是一个简单的入门级练习，用于熟悉 `Option` 的基本使用方法，包括如何创建 `Some` 和 `None` 值，以及如何使用 `match` 语句和其他方法来处理 `Option` 值。

### 练习 49 (`options2`)

这个练习要求你处理一个包含字符串和命令的列表，命令决定了对字符串的操作（如大写、修剪、附加字符串）。你将需要使用 `Option` 来处理可能的空值，并探索 `if let` 和 `while let` 等 Rust 的控制流构造，这些构造允许你在更少的代码行中优雅地处理 `Option` 值。

### 练习 50 (`options3`)

在 `options3` 练习中，你将遇到一个编译器报错，提示你在 `match` 语句中部分移动了一个值。这个练习引导你了解如何避免这种情况，特别是如何在需要时使用引用（`ref` 关键字）来访问 `Option` 内部的值，而不会取得其所有权，这是理解 Rust 所有权和借用规则的重要一步。

通过完成这些练习，你不仅将加深对 `Option` 枚举的理解，还将学习到 Rust 中关于所有权、借用、模式匹配等核心概念的高级用法。这些练习将帮助你建立处理 Rust 中可选值和错误处理的坚实基础，为后续的 Rust 学习之旅铺平道路。

---

```rust
// 你不必现在理解以下代码，不过你可以尝试运行它。

enum Command {
    Uppercase,
    Trim,
    Append(String),
}

fn main() {
    let clean_water_availability = maybe_clean_water(20);
    match clean_water_availability {
        Some(quantity) => println!("Found {} drops of clean water!💧", quantity),
        None => println!("No clean water found at this time."),
    }

    let commands = vec![Command::Uppercase, Command::Trim, Command::Append(" in the forest".to_string())];
    let mut message = Some("You hear a whisper".to_string());
    for command in commands {
        message = apply_command(message, command);
    }
    println!("Message transformed: {:?}", message);

    println!("\nYour journey continues into the dark forest.🌲🌲🌲🌲🌲");
}

fn maybe_clean_water(hour: u8) -> Option<u8> {
    match hour {
        0..=6 => Some(4),
        19..=22 => Some(2), 
        _ => None,
    }
}

fn apply_command(current: Option<String>, command: Command) -> Option<String> {
    match current {
        Some(mut text) => match command {
            Command::Uppercase => {
                text = text.to_uppercase();
                Some(text)
            }
            Command::Trim => {
                text = text.trim().to_string();
                Some(text)
            }
            Command::Append(extra) => Some(text + &extra),
        },
        None => None,
    }
}

```
