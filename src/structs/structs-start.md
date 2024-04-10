# structs

---

在即将开始的一系列`structs`练习中，我们将深入探索Rust中结构体的丰富用法。结构体是Rust中一种将数据聚合在一起的主要工具，它不仅能够存储数据，还可以附加逻辑。通过这些练习，你将学会如何定义和使用不同类型的结构体，以及如何为它们实现方法。

从`structs1`练习开始，你将遇到三种不同类型的结构体：普通结构体、元组结构体和类单元结构体。这些结构体分别用于不同的场景，例如，普通结构体用于存储具有名称的字段，元组结构体类似于元组但具有结构体的性质，而类单元结构体则常用于实现特质而不存储任何数据。

在`structs2`练习中，你将学习到如何更高效地创建结构体实例，特别是当你需要基于一个现有实例创建新实例时，Rust提供的结构体更新语法将会非常有用。

到了`structs3`练习，我们将进一步探索结构体的逻辑部分，通过在结构体上定义方法来处理更复杂的逻辑，如计算费用和判断包裹是否为国际邮寄等。这些方法展示了如何将数据和逻辑封装在一起，使得你的Rust程序更加模块化和易于管理。

在完成这些练习的过程中，你会发现Rust对于数据管理的独特思考，以及它如何帮助你编写更安全、更可靠的代码。每个练习都是精心设计的，旨在帮助你逐步掌握结构体的使用，并鼓励你思考如何在自己的项目中有效地利用它们。

这些练习后面还有更多的内容等待你的探索，每一步都将加深你对Rust编程语言的理解。开始吧享受这个过程，很快你就会发现自己在使用Rust构建强大且优雅的解决方案上变得越来越自信。

---

```rust
// 你不必现在理解以下代码，不过你可以尝试运行它。

struct Explorer {
    name: String,
    lanterns: u8,
}

struct Coordinates(i32, i32);

struct MysteriousFog;

fn main() {
    let explorer = Explorer {
        name: "Ferris the Fearless".to_string(),
        lanterns: 3,
    };

    let starting_point = Coordinates(0, 0);
    let new_explorer = Explorer {
        name: "Luna the Lightfooted".to_string(),
        ..explorer
    };

    explorer.shine_lantern();
    new_explorer.shine_lantern();
    println!("Setting off from coordinates: ({}, {})", starting_point.0, starting_point.1);

    encounter_fog(MysteriousFog);
    println!("\nYour journey continues into the dark forest.🌲🌲🌲");
}

impl Explorer {
    fn shine_lantern(&self) {
        println!("{} shines their lantern, revealing hidden paths and secrets.", self.name);
        for _ in 0..self.lanterns {
            println!("A lantern glows brightly, piercing the darkness. 🌲");
        }
    }
}

fn encounter_fog(_fog: MysteriousFog) {
    println!("A mysterious fog envelops you, dampening sound and distorting shapes.");
}

```





