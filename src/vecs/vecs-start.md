# vecs

---

欢迎进入Rust编程之旅中的一个新的里程碑——向量（Vectors）。向量是Rust中非常强大且灵活的数据结构，它允许你在单一数据结构中存储多个值。这些值可以在运行时增加或减少，提供了比静态数组更多的动态性和灵活性。在即将开始的章节中，我们将通过两个练习来深入探索向量的使用和操作。

1. **创建和初始化向量（`vecs1`）**：在这个练习中，你将学习如何创建一个向量，并将其初始化为包含与给定数组相同元素的集合。这个任务将帮助你理解向量的声明和初始化语法，以及如何利用向量存储一系列值。

2. **向量的遍历和转换（`vecs2`）**：继续深入，下一个练习将教会你如何遍历向量中的元素，并对每个元素执行操作。你将体验到两种不同的方法：直接在遍历时修改元素的值，以及使用`map`函数创建一个新的转换后的向量。这两种方法展示了Rust提供的灵活性，让你可以根据具体情况选择最合适的解决方案。

通过这些练习，你不仅将掌握向量的基本操作，如创建、初始化、遍历和转换，还将深入了解Rust的迭代器和闭包等高级特性。向量是Rust中用于处理数据集合的基础工具，无论是在简单的数据收集还是在复杂的算法实现中，都发挥着不可替代的作用。

让我们一起开始向量章节的学习，探索Rust中这个强大的内容吧！

---

```rust
// 你不必现在理解以下代码，不过你可以尝试运行它。

fn main() {
    let initial_items = [ '🔑', '🔮'];
    let mut magical_items: Vec<char> = Vec::from(initial_items);
    println!("You have collected magical items: {:?}", magical_items);

    magical_items.push('📖');
    println!("After exploring the ancient ruins, your collection of magical items grows: \n{:?}", magical_items);

    for item in &mut magical_items {
        *item = match *item {
			'🔑' => '🚪',
            '🔮' => '💎',
            '📖' => '🧙',
            _ => *item,
        };
    }
    println!("Transformed magical items: {:?}", magical_items);

    let transformed_items: Vec<String> = magical_items.into_iter().map(|item| {
        match item {
			'🚪' => "a mysterious door that leads to unknown realms".to_string(),
            '💎' => "a gem that shines with an inner light".to_string(),
            '🧙' => "a mage who offers his wisdom".to_string(),
            _ => "an unknown item".to_string(),
        }
    }).collect();

    println!("The magic in the items reveals their true forms:");
    for item_description in transformed_items {
        println!(" - {}", item_description);
    }

    println!("\nYour journey continues into the dark forest.\n🌲🌲🌲🌲🌲🌲🌲");
}

```

