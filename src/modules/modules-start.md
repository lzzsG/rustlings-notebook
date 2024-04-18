# modules

---

在Rust中，模块系统是组织和封装代码的核心。通过这一系列练习，你将深入了解如何定义模块、如何控制函数和类型的可见性，以及如何从标准库或其他模块导入项。下面是对最近的`modules`练习的介绍，旨在为你开始这一部分的学习旅程提供一个清晰的起点。

### 模块1 - 公开模块项

在`modules1`练习中，你将面临如何使模块内的函数对外部可见的挑战。Rust默认情况下，所有的项都是私有的。这个练习将教会你如何使用`pub`关键字来公开模块中的项，使得它们可以被模块外的代码所访问。

### 模块2 - 重导出与别名

`modules2`练习进一步探讨了模块的强大功能，特别是如何通过`use`和`as`关键字为模块路径提供新的名称，以及如何使用`pub use`来重导出项。这不仅可以简化外部对模块项的访问，还允许你在不改变内部实现的前提下，向外部提供一个不同于内部结构的接口。

### 模块3 - 从标准库导入

在`modules3`练习中，你将练习从Rust标准库导入项。这个练习特别指向了`std::time`模块，要求你导入`SystemTime`和`UNIX_EPOCH`。这将加深你对如何使用`use`语句从标准库或其他库导入项的理解，同时也是一个探索Rust标准库的好机会。

通过这些练习，你不仅会学会如何在自己的项目中有效地使用模块来组织代码，还会了解到模块如何在Rust生态中发挥作用，尤其是在构建大型和复杂项目时。掌握模块的使用，是成为一名熟练Rust开发者的重要一步。每个练习都旨在加深你对模块系统的理解，为你提供必要的知识去构建更清晰、更健壮的Rust应用。

---

```rust
// 你不必现在理解以下代码，不过你可以尝试运行它。

mod magic {
    pub fn ancient_spell() {
        println!("Prepare to cast an ancient space-time transformation spell...\n\"Ha ze ro ri a ru\"\n\"Ha ji ke ro si na pu su\"\n\"Banishiment this world!\"");
    }

    pub mod special {
        pub fn teleport(destination: &str) {
            println!("Teleporting to {}...", destination);
        }
    }
    
    pub use self::special::teleport;
}

use magic::{ancient_spell, teleport};
use std::collections::HashMap;

fn main() {
    ancient_spell();

    let mut treasures = HashMap::new();
    treasures.insert("'Golden Statue'", "Deep in the haunted tomb");
    treasures.insert("'Lost AI'", "Lost city ruins");
	treasures.insert("'Wild Kitty'", "Roadside");
    for (treasure, location) in &treasures {
        println!("Found {} at {}", treasure, location);
    }

    teleport("'Old Summer'");

    println!("\nYour journey continues into the dark forest.🌲🌲\nThe ground beneath your feet is a patchwork of shadows and decaying leaves, \nthe air chilled and heavy with the scent of moss and something unnameable.🍃");
}
```
