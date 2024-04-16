# Exercise 86

- Name: ```macros3```
- Path: ```exercises/macros/macros3.rs```
#### Hint: 

In order to use a macro outside of its module, you need to do something special to the module to lift the macro out into its parent.

The same trick also works on "extern crate" statements for crates that have exported macros, if you've seen any of those around.


---



```rust,editable
// macros3.rs
//
// Make me compile, without taking the macro out of the module!
//
// Execute `rustlings hint macros3` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

mod macros {
    macro_rules! my_macro {
        () => {
            println!("Check out my macro!");
        };
    }
}

fn main() {
    my_macro!();
}

```

---

### 本题内容

这个练习的目的是理解如何在 Rust 中跨模块使用宏。Rust 提供了特定的属性和规则来控制宏的可见性和作用域，这个练习将帮助学习者掌握这些技巧，以便在大型项目中有效地组织和重用宏。

### 相关知识点

1. **宏的作用域**：
   - 宏默认只在定义它们的模块内可见。如果你想在模块外部使用宏，你需要使用 `#[macro_export]` 属性来显式导出它们。

2. **导出宏**：
   - 使用 `#[macro_export]` 属性可以使宏跨模块可见。这个属性告诉编译器，即使这个宏定义在一个模块内，它也应该在模块外部可用。

3. **模块系统**：
   - Rust 的模块系统帮助组织和封装代码。理解如何在模块间正确引用函数、变量和宏是高效使用 Rust 的关键。

### 解题方法

#### 步骤描述

1. **添加宏导出属性**：
   - 在模块内部的宏定义前添加 `#[macro_export]` 属性。这将使宏可以在模块外部使用。

2. **调用宏**：
   - 在 `main` 函数中调用宏，验证是否可以正确识别和执行。

3. **编译和测试**：
   - 编译程序，确保没有编译错误。
   - 运行程序，检查输出是否符合预期。

#### 代码示例

正确修改后的代码示例：

```rust
// macros3.rs

mod macros {
    #[macro_export]
    macro_rules! my_macro {
        () => {
            println!("Check out my macro!");
        };
    }
}

fn main() {
    my_macro!();
}
```

通过这个练习，你将学习到如何在 Rust 中跨模块使用宏。掌握如何导出和引用宏是编写清晰、可维护和可重用 Rust 代码的重要技能。这种能力在开发大型或复杂的 Rust 应用程序时尤其重要，因为它可以帮助你更好地组织代码和逻辑。通过合理使用 `#[macro_export]`，你可以确保宏在需要时能够正确地被其他部分的代码调用。

## 扩展知识点与解答：

### 扩展知识点

1. **宏可见性和作用域管理**：
   - 宏的作用域管理与函数略有不同，因为宏的展开发生在编译时，而不是运行时。理解宏的作用域规则和如何通过 `#[macro_export]` 属性控制宏的可见性是深入理解 Rust 宏系统的关键部分。

2. **全局宏与局部宏**：
   - 使用 `#[macro_export]` 声明的宏在其定义的包中是全局可用的。这对于创建通用库特别有用，这些库提供可以在多个项目中使用的宏。
   - 不带 `#[macro_export]` 的宏只在其定义的当前模块（和子模块）内可见，这种封装有助于避免命名冲突，并确保宏的使用按照模块的设计来进行。

3. **宏重载**：
   - Rust 的宏系统支持重载，允许定义多个模式以匹配不同的输入。这提供了高度的灵活性，使得宏可以根据输入参数的不同而展开为不同的代码。

### 扩展解题方法

1. **宏的逐步开发**：
   - 开发复杂的宏时，建议从简单的实现开始，逐步添加更多功能。通过逐步构建和频繁测试，可以更容易地识别和修复在宏展开过程中出现的错误。

2. **使用宏进行测试驱动开发（TDD）**：
   - 尽管宏可能难以直接测试，但可以通过编写使用这些宏的函数或结构体的测试来间接测试宏。这种方法符合测试驱动开发的理念，可以确保宏按预期工作。

3. **宏的文档和示例**：
   - 为宏编写充分的文档和使用示例至关重要，尤其是当宏用于公共API时。Rust 的文档注释支持示例代码，这些示例代码可以作为编译时测试自动执行，确保文档的准确性和宏的稳定性。

4. **宏的调试技巧**：
   - 调试宏可能具有挑战性，因为错误通常隐藏在复杂的宏展开中。除了使用 `cargo expand` 查看宏展开的代码外，逐步简化宏定义，直到找到引起问题的部分，是一种有效的调试方法。

通过掌握这些扩展知识点和解题方法，你可以更有效地使用 Rust 的宏来构建复杂和高性能的应用程序，同时确保代码的可维护性和可扩展性。
