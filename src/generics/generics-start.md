# generics

---

在这一系列的泛型（Generics）练习中，我们将一步步深入探讨 Rust 中泛型的强大功能。通过这些练习，你将学会如何使用泛型来创建更通用、更灵活的代码，这对于构建复杂的数据结构和功能强大的函数至关重要。

### 泛型的基本概念

泛型是 Rust 强类型系统的一个核心部分，它允许你编写能够处理多种数据类型的函数和数据结构，而不需要为每种可能的数据类型编写重复的代码。这不仅可以提高代码的重用性，还能提升代码的清晰度和安全性。

### 开始之前

- **泛型和向量**：你将了解到 Rust 中的向量（`Vec<T>`）是如何使用泛型来存储任意类型数据的。这是泛型应用的一个简单示例，帮助你理解泛型参数如何在实际编程中使用。

- **自定义泛型数据结构**：随着对泛型的深入理解，你将学会如何定义能够存储任意类型数据的自定义结构体。这是理解和应用泛型的关键一步，也是构建灵活且通用库的基础。

### 练习概览

- **generics1**：这个练习将引导你解决一个关于向量类型推断的问题。你需要修正代码，以正确地声明一个存储特定类型数据的向量。
  
- **generics2**：在这个练习中，你将面对一个更具挑战性的任务——修改一个只能存储 `u32` 类型数据的结构体，使其能够使用泛型存储任意类型的数据。这不仅会测试你对泛型基础的理解，还会挑战你将泛型应用于实际数据结构的能力。

### 学习目标

完成这些练习后，你将能够：

- 理解泛型的基本概念和作用。
- 使用泛型参数声明和实现通用函数和数据结构。
- 理解如何通过泛型提高代码的复用性和灵活性。

泛型是 Rust 中一个非常强大的特性，掌握它可以大大提高你的 Rust 编程技能。现在，让我们开始这些练习，探索泛型的奥秘吧！

---

```rust
// 你不必现在理解以下代码，不过你可以尝试运行它。

use std::any::Any;

enum Item {
    Integer(i32),
    Float(f64),
    Text(String),
}

struct MagicBag {
    items: Vec<Box<dyn Any>>,
}

impl MagicBag {
    fn new() -> Self {
        MagicBag { items: Vec::new() }
    }

    fn add<T: 'static + Any>(&mut self, item: T) {
        self.items.push(Box::new(item));
    }

    fn show_contents(&self) {
        for item in &self.items {
            if let Some(integer) = item.downcast_ref::<i32>() {
                println!("🌲An integer: {}", integer);
            } else if let Some(float) = item.downcast_ref::<f64>() {
                println!("🌲A float: {}", float);
            } else if let Some(text) = item.downcast_ref::<String>() {
                println!("🌲A piece of text: {}", text);
            } else {
                println!("🌲An item of unknown type");
            }
        }
        println!("🌲Your magic bag contains {} items.", self.items.len());
    }
}

fn main() {
    let mut bag = MagicBag::new();

    bag.add(42); 
    bag.add(3.1415926535); 
    bag.add("A mysterious rune".to_string());
    bag.add("🗄️"); 

    bag.show_contents();

    println!("\n🌲Your journey continues into the dark forest.🌲");
}

```
