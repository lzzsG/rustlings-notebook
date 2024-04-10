# functions

---

在即将开始的章节中，我们将深入探索Rust中的函数——这是构建和组织Rust程序的基础。通过一系列的练习，你将了解到函数如何定义、如何接受参数、以及如何返回值。函数不仅帮助我们封装和重用代码，还是实现程序逻辑、表达算法思想的关键工具。

1. **函数定义和调用**：我们将从如何定义一个最基本的函数开始，探索Rust中函数的基本语法。你将学习到如何调用一个函数，以及为何在Rust中调用函数是如此重要。

2. **带参数的函数**：随后，我们会深入探讨如何给函数传递参数。通过实践，你将理解到参数如何增强函数的灵活性和复用性，以及如何在函数间传递信息。

3. **函数返回值**：接下来，我们将学习函数如何返回值。你将看到，在Rust中，函数的返回值是如何被明确指定的，以及它们如何被用来传递计算结果。

4. **函数签名和类型**：通过一系列的练习，我们将探索函数签名的重要性，包括参数类型和返回类型。这将帮助你理解Rust的类型系统如何作用于函数，以及如何通过类型来确保函数的正确性。

5. **复合类型的函数参数**：我们还将研究如何将复合类型（如元组、数组等）作为参数传递给函数，以及如何从函数中返回复合类型。这将进一步拓展你对Rust函数的理解。

通过这些练习，你将建立起对Rust函数的全面理解，包括它们的定义、使用和在Rust程序中的角色。你将学会如何设计清晰、高效、易于维护的函数，这是成为一名优秀Rust开发者的关键步骤。让我们一起开始这一段旅程吧！

---

```rust
// 你不必现在理解以下代码，不过你可以尝试运行它。

fn main() {
    let challenge = encounter("a deep, dark cave filled with unknown dangers");
    println!("You have encountered {} 🦇.", challenge);
    
    let puzzles = [("glyphs", 4), ("runes", 7), ("totems", 3)];
    let clues_found = solve_puzzles(&puzzles);
    println!("You solve the puzzles of encounters and find clues: \nglyphs: {}, runes: {}, totems: {} 🗝️.", clues_found.0, clues_found.1, clues_found.2);

    let wisdom = gain_wisdom(5);
    println!("Through your experiences, you've gained {} wisdom points.", wisdom);

    let has_artifact = find_artifact(false);
    if has_artifact {
        println!("You have found the Ancient Artifact!");
    } else {
        println!("The Ancient Artifact remains elusive.");
    }

    let next_step = decide_next_step("a narrow, mysterious path leading deeper into the forest");
    println!("You decide to take {}", next_step);

    let journey_end = continue_journey();
    println!("{}", journey_end);
}

fn encounter(obstacle: &str) -> String {
    format!("{}.", obstacle)
}

fn solve_puzzles(puzzles: &[(&str, i32)]) -> (i32, i32, i32) {
    let mut glyphs_clues = 0;
    let mut runes_clues = 0;
    let mut totems_clues = 0;

    for puzzle in puzzles {
        match *puzzle {
            ("glyphs", complexity) => glyphs_clues += complexity,
            ("runes", complexity) => runes_clues += complexity,
            ("totems", complexity) => totems_clues += complexity,
            _ => (),
        }
    }

    (glyphs_clues, runes_clues, totems_clues)
}


fn gain_wisdom(points: i32) -> i32 {
    points
}

fn find_artifact(found: bool) -> bool {
    found
}

fn decide_next_step(path: &str) -> String {
    format!("{}", path)
}

fn continue_journey() -> String {
    "\nYour journey continues into the dark forest. \n🌲The Ancient Artifact🌲still somewhere🌲out there🌲, waiting to be discovered. \n".to_string()
}

```

