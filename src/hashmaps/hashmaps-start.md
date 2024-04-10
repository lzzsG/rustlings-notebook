# hashmaps

---

一系列的`hashmaps`练习旨在通过实际的例子来加深你对Rust中`HashMap`使用的理解。通过这些练习，你将学会如何创建、更新和有效地管理`HashMap`数据结构。以下是练习的简要概述和它们所覆盖的关键概念：

### 练习44（`hashmaps1`）

这个练习引导你创建一个代表水果篮的`HashMap`，使用水果名作为键，数量作为值。你需要至少添加三种不同类型的水果，确保总数至少为五。这个练习教会了如何初始化`HashMap`并向其中插入数据。

### 练习45（`hashmaps2`）

在这个练习中，你将使用`entry()`和`or_insert()`方法来只在键不存在时插入新值。这是`HashMap`提供的一种强大的数据更新和管理策略，可帮助你避免覆盖已有的数据。

### 练习46（`hashmaps3`）

这个练习更进一步，不仅要求你插入数据，还要求根据现有值更新数据。使用`entry()`方法获取到的条目可以根据旧值来更新。这对于累加计数或根据旧值计算新值非常有用。

通过完成这些练习，你应该对如何使用`HashMap`来处理更复杂的数据更新和管理任务有了更深入的了解。`HashMap`是Rust标准库中非常强大的工具之一，能够帮助你高效地管理和更新键值对数据集合。了解`entry()`和`or_insert()`方法的使用，可以让你更加灵活和高效地处理数据更新场景，避免不必要的重复检查和数据覆盖，从而编写出更清晰、更高效的Rust代码。

---

```rust
// 你不必现在理解以下代码，不过你可以尝试运行它。

use std::collections::HashMap;

fn main() {
    let mut herb_basket = HashMap::new();
    herb_basket.insert("Glowleafi", 3);
    herb_basket.insert("Moonflower", 2);
    herb_basket.insert("Sunroot", 5);

    for (herb, quantity) in &herb_basket {
        println!("You have collected {} {}s.", quantity, herb);
    }

    herb_basket.entry("Starweed").or_insert(2);
    println!("\nAfter a mysterious encounter, your basket now contains:");
    for (herb, quantity) in &herb_basket {
        println!("{} {}", quantity, herb);
    }

    let additional_herbs = vec![("Moonflower", 3), ("Glowleaf", 1), ("Sunroot", 1)];
    for (herb, quantity) in additional_herbs {
        let entry = herb_basket.entry(herb).or_insert(0);
        *entry += quantity;
    }
    println!("\nAfter trading with a wandering alchemist, your basket now has:");
    for (herb, quantity) in &herb_basket {
        println!("{} {}", quantity, herb);
    }

    println!("\n🍃Your journey continues into the dark forest.🌲🌲🌲");
}

```
