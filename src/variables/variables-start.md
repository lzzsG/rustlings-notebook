# variables

---

欢迎来到Rust世界的第一站：变量。在这个章节中，我们将通过一系列精心设计的练习，深入探索Rust中的变量声明、修改以及它们在Rust语言中扮演的角色。变量是编程语言中最基本的组成部分之一，理解如何在Rust中有效地使用变量，对于学习和掌握这门语言至关重要。

1. **变量声明**：我们将从最基础的变量声明开始，了解Rust中变量如何被创建和初始化。通过解决编译时错误，你将学习到Rust的变量默认是不可变的，以及如何通过关键字`let`来声明变量。

2. **变量初始化**：接着，我们会探讨变量的初始化问题。Rust强调安全性，未初始化的变量是不能使用的。我们将实践如何正确初始化变量，以及Rust是如何通过编译时检查来防止潜在的空值错误。

3. **变量类型和可变性**：随后的练习将带你深入理解Rust中的变量可变性和类型系统。你将学到如何通过`mut`关键字使变量可变，以及为何在某些情况下明确指定变量类型是有必要的。

4. **变量值的修改**：我们还将探索在Rust中如何修改变量的值。通过对可变变量进行重新赋值，你将理解Rust中变量可变性的核心概念，以及它如何帮助你写出更安全、更清晰的代码。

5. **变量遮蔽**：Rust提供了一个有趣的特性——变量遮蔽，允许你在相同作用域内用新的值“遮蔽”之前声明的变量。这一特性在处理需要转换值类型或临时改变变量值的场景中非常有用。

6. **常量**：最后，我们会介绍Rust中的常量。常量与变量相似，但它们一旦被赋值后就无法改变。你将学习到常量的声明方式，以及它们与变量的区别和使用场景。

通过这些练习，你不仅会对Rust中的变量有一个全面的了解，还会对Rust的安全性和效率特性有更深刻的认识。让我们一起开始探索Rust变量的奥秘吧！

---

```rust
// 你不必现在理解以下代码，不过你可以尝试运行它。

fn main() {
    let your_name = "Ferris the Brave"; 
    println!("Your name is {}, and your journey has begun.", your_name);

    let mut dangers_faced = 0;
    println!("You, {}, are ready to face any challenge that comes your way.", your_name);

    dangers_faced += 1; 
    println!("You encountered a wild beast, increasing the dangers you've faced to {}.", dangers_faced);

    let your_name = "Ferris the Wise"; 
    println!("After defeating the beast, you, {}, learn from the experience and \nbecome wiser.", your_name);

    const MAX_DANGERS: u32 = 110;
    println!("You, {}, know there are up to {} challenges before reaching the \nancient artifact.", your_name, MAX_DANGERS);

    let dangers_faced = dangers_faced + 1;
    let artifact_status = if dangers_faced > MAX_DANGERS {
        "with the ancient artifact securely in your grasp"
    } else {
        "but your journey is far from over"
    };

    println!("You emerge from the battle {}. \n\nYour journey continues into the dark forest.🌲🌲🌲", artifact_status);
}

```

