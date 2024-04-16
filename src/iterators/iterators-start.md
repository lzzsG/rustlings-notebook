# iterators

---

在 Rust 编程语言中，迭代器是处理集合数据的强大工具，允许你以高效和表达性强的方式进行数据遍历和转换。以下是一系列专门针对迭代器使用的练习，通过这些练习，你将深入理解迭代器的各种操作，并应用它们来解决实际问题。

#### 迭代器练习 1：迭代器基础

在这个练习中，你将通过填充代码来创建和使用迭代器，学习如何从一个简单的字符串数组开始，使用迭代器逐步访问每个元素。这是学习迭代器操作的基础，帮助你理解如何在 Rust 中开始迭代过程，并掌握迭代器的基本使用方式。

#### 迭代器练习 2：字符串首字母大写

此练习将引导你使用迭代器对字符串数组中的每个元素进行处理，具体是将每个字符串的首字母转换为大写。你将学习如何结合迭代器和字符串处理功能，以函数式编程的方式改造和简化代码。

#### 迭代器练习 3：复杂的错误处理

在这个较为复杂的练习中，你将处理可能发生错误的运算，学习如何使用迭代器管理错误。通过实现一个除法函数并处理其可能的错误，你需要使用迭代器来收集操作结果，这将涉及到对迭代器更高级的使用，如 `collect` 方法及其与 `Result` 类型的结合。

#### 迭代器练习 4：计算阶乘

这个练习挑战你使用迭代器来计算阶乘，不使用传统的循环或递归方法。你将探索如何使用 `fold` 或其他迭代器方法来累计值，这是一个非常好的实践，可以加深你对迭代器如何在背后处理数据的理解。

#### 迭代器练习 5：高级数据统计

最后一个练习将要求你统计一系列散列表（HashMap）中的数据，这些散列表记录了某些项目的完成状态。你将使用迭代器来遍历并统计特定状态的出现次数，这不仅测试了你对迭代器的理解，还包括了对更复杂数据结构的处理。

通过这些练习，你将获得使用 Rust 迭代器进行有效编程的实际经验，学会如何利用它们解决具体问题，并优化你的代码结构和性能。

---

```rust
// 你不必现在理解以下代码，不过你可以尝试运行它。

use std::collections::HashMap;

struct Adventure {
    challenges: Vec<String>,
    secret_codes: HashMap<String, i32>,
}

impl Adventure {
    fn new() -> Self {
        Adventure {
            challenges: vec![
                "decode the ancient text".to_string(),
                "unravel the secret of the stones".to_string(),
                "find the hidden path".to_string(),
            ],
            secret_codes: HashMap::new(),
        }
    }

    fn begin_journey(&mut self) {
        for challenge in self.challenges.iter() {
            println!("Challenge: {}", challenge);
        }

        let mut modified_challenges: Vec<String> = self.challenges.iter()
            .map(|c| c.chars().next().unwrap().to_uppercase().collect::<String>() + c.split_at(1).1)
            .collect();
        
        println!("Modified Challenges: {:?}", modified_challenges);

        self.secret_codes.insert("Decode the ancient text".to_string(), 9);
        let result: Result<Vec<_>, _> = self.secret_codes.iter()
            .map(|(key, &value)| {
                if key == "Decode the ancient text" && value == 9 {
                    Ok(value)
                } else {
                    Err("Secret code mismatch")
                }
            })
            .collect();

        match result {
            Ok(codes) => println!("Collected secret codes: {:?}", codes),
            Err(e) => println!("Error encountered: {}", e),
        }

        let factorials: Vec<_> = (1..=9).map(|n| (1..=n).fold(1, |acc, i| acc * i)).collect();
        println!("Calculated: {:?}", factorials);

        let count = modified_challenges.iter().filter(|&m| m.contains("the")).count();
        println!("Count of secrets: {}", count);
    }
}

fn main() {
    let mut adventure = Adventure::new();
    adventure.begin_journey();

    println!("\n🌲Your journey continues into the dark forest.🌲🌲");
}
```
