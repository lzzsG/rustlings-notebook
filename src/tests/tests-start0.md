# tests

---

在这个系列的 Rust 练习中，我们聚焦于学习如何通过测试来验证代码的正确性，确保它按照预期工作。以下是三个针对不同测试技术的练习概览，每个练习都有其独特的学习目标和方法。

### 练习 68: tests1
在这个练习中，你将使用 `assert!` 宏来学习如何进行基本的断言测试。这种测试对于验证代码中的布尔条件非常重要。通过这个练习，你可以体验到编写测试代码确保函数逻辑正确性的重要性。

#### 学习要点：
- **`assert!` 宏的使用**：这个宏用来检查一个条件是否为真，如果为假则引发 panic，这是一种基础的测试技巧。

### 练习 69: tests2
这个练习进一步引导你使用 `assert_eq!` 宏，它用于比较两个表达式的结果是否相等。你将通过实际的例子来理解这个宏如何帮助确保程序各部分按预期工作。

#### 学习要点：
- **比较与断言**：学习如何用 `assert_eq!` 宏来确保两个值相等，这在单元测试中尤为重要，尤其是在处理函数返回值或计算结果时。

### 练习 70: tests3
本练习将介绍如何测试一个简单的数学函数 `is_even`，这是一个判断整数是否为偶数的函数。你需要写出两个测试：一个验证偶数返回 `true`，另一个验证奇数返回 `false`。

#### 学习要点：
- **布尔逻辑测试**：通过 `assert!` 或 `assert_eq!` 测试布尔值，这对于验证任何返回布尔类型的函数非常有用。

### 练习 71: tests4
在这个练习中，你需要处理的是异常情况测试——确保 `Rectangle` 类在接收到不合理的输入（如负数的宽度或高度）时正确地引发 panic。

#### 学习要点：
- **异常处理**：使用 `should_panic` 属性测试异常处理，这对于验证函数在面对错误输入时是否能够按预期失败非常关键。

这些练习都是为了帮助你掌握 Rust 中的测试技巧，使你能够编写更稳健、可靠的 Rust 代码。通过学习如何测试正常的逻辑、边界条件以及异常情况，你可以确保你的代码在各种情况下都能正确运行。

---

```rust
// 你不必现在理解以下代码，不过你可以尝试运行它。

struct MagicPotion {
    potency: i32,
}

impl MagicPotion {
    fn new(potency: i32) -> Self {
        if potency < 0 {
            panic!("Magic potion potency cannot be negative!");
        }
        MagicPotion { potency }
    }

    fn is_powerful(&self) -> bool {
        self.potency > 50
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_magic_potion_creation() {
        let potion = MagicPotion::new(10);
        assert_eq!(potion.potency, 10, "Potion potency should be equal to the value passed to new.");
    }

    #[test]
    #[should_panic(expected = "Magic potion potency cannot be negative!")]
    fn test_negative_potency() {
        let _potion = MagicPotion::new(-10);
    }

    #[test]
    fn test_potion_power() {
        let weak_potion = MagicPotion::new(20);
        assert!(!weak_potion.is_powerful(), "Potion should not be powerful.");
        
        let powerful_potion = MagicPotion::new(60);
        assert!(powerful_potion.is_powerful(), "Potion should be powerful.");
    }
}

fn main() {
    println!("Elric the Wise continues his journey, armed with his knowledge of the arcane arts.🪄");

    println!("\nYour journey continues into the dark forest.🌲\nSinister whispers float through the air, a language unknown yet unsettlingly familiar,\nas if the forest itself is alive and watching.");
}
```

