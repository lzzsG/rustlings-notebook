# Exercise 43

- Name: ```modules3```
- Path: ```exercises/modules/modules3.rs```
#### Hint: 

UNIX_EPOCH and SystemTime are declared in the std::time module. Add a use statement
for these two to bring them into scope. You can use nested paths or the glob
operator to bring these two in using only one line.


---



```rust,editable
// modules3.rs
//
// You can use the 'use' keyword to bring module paths from modules from
// anywhere and especially from the Rust standard library into your scope. Bring
// SystemTime and UNIX_EPOCH from the std::time module. Bonus style points if
// you can do it with one line!
//
// Execute `rustlings hint modules3` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

// TODO: Complete this use statement
use ???

fn main() {
    match SystemTime::now().duration_since(UNIX_EPOCH) {
        Ok(n) => println!("1970-01-01 00:00:00 UTC was {} seconds ago!", n.as_secs()),
        Err(_) => panic!("SystemTime before UNIX EPOCH!"),
    }
}

```

---

### 本题内容：

这个练习的目标是从Rust标准库中导入`SystemTime`和`UNIX_EPOCH`。`use`语句可以帮助你将这两个项从`std::time`模块导入当前作用域，从而在`main`函数中使用它们。本题旨在加深你对如何从Rust标准库导入项的理解，以及如何通过简洁的方式完成导入。

### 相关知识点：

- **`use`语句**：用于将特定路径下的项导入当前作用域，简化后续的代码引用。

- **嵌套路径**：Rust 2018 edition引入的特性，允许在一个`use`语句中通过大括号`{}`导入同一模块下的多个项。

- **通配符**：`*`，也称为glob运算符，用于导入一个路径下的所有项。虽然方便，但为了保持代码的清晰，推荐在确定需要导入大量项且不影响可读性的情况下使用。

### 解题方法：

#### 使用嵌套路径或通配符导入
- **步骤描述**：
  1. 使用嵌套路径导入`SystemTime`和`UNIX_EPOCH`：`use std::time::{SystemTime, UNIX_EPOCH};`
  2. 或使用通配符导入`std::time`模块下的所有项：`use std::time::*;`

#### 代码示例：

```rust
// 使用嵌套路径导入SystemTime和UNIX_EPOCH
use std::time::{SystemTime, UNIX_EPOCH};

fn main() {
    match SystemTime::now().duration_since(UNIX_EPOCH) {
        Ok(n) => println!("1970-01-01 00:00:00 UTC was {} seconds ago!", n.as_secs()),
        Err(_) => panic!("SystemTime before UNIX EPOCH!"),
    }
}
```

通过完成这个练习，你将了解到如何高效地从Rust标准库或其他模块导入多个项。熟悉`use`语句及其高级用法是构建大型Rust应用时的重要技能，它能帮助你保持代码的整洁和可维护性。此外，理解标准库中各种模块和类型的组织结构也对于深入学习Rust至关重要。

## 扩展知识点与解答：

完成`modules3`练习后，你已经掌握了如何从Rust标准库导入项，并且可能已经开始欣赏到`use`语句在简化代码中的作用。以下是与本题相关的扩展知识点和解题方法，这些将帮助你进一步理解和掌握Rust模块系统的强大功能。

### 扩展知识点：

1. **模块路径的重导出**：
   - 使用`pub use`语句可以重导出模块路径，这允许你重新组织模块结构，而不破坏现有代码。重导出特别适用于创建库时，当你想暴露一个清晰简洁的公共API，同时隐藏内部实现细节。

2. **预导入（Prelude）模块**：
   - 标准库和一些外部库提供了所谓的prelude模块，这是一个包含了该库最常用的项的特殊模块。通过在crate根导入这些prelude模块，可以减少需要显式导入的常用类型和特征数量。

3. **限定名称与非限定名称**：
   - `use`语句导入的项可以以限定名称（带有模块前缀）或非限定名称（不带模块前缀）的形式使用。理解何时使用限定名称可以帮助提高代码的清晰度和避免名称冲突。

### 扩展解题方法：

1. **构建模块树**：
   - 尝试为你的项目构建一个模块树，使用文件夹和`mod.rs`文件来组织你的模块。理解如何利用Rust的文件系统规则来映射模块结构是构建大型项目的关键。

2. **探索标准库**：
   - 利用本练习中使用的`SystemTime`和`UNIX_EPOCH`作为起点，探索标准库中的其他模块和项。尝试导入和使用标准库中的不同类型和函数，了解它们的用途和功能。

3. **深入学习路径简写**：
   - 实践使用路径简写技术，比如使用嵌套路径来一次性导入一个模块中的多个项。了解何时使用路径简写可以增加你代码的可读性和简洁性。

#### 示例代码：

```rust
// 重导出示例
pub mod api {
    pub use crate::internal::implementation;
}

// 在一个库的prelude模块中
pub mod prelude {
    pub use crate::{SomeType, some_function};
}

// 使用限定名称导入和使用
use std::collections::HashMap;

fn example() {
    let map: HashMap<String, i32> = HashMap::new();
}
```

通过掌握这些扩展知识点和解题方法，你将能够更加有效地使用Rust的模块系统，构建结构清晰、易于维护的Rust应用程序和库。Rust模块系统的设计旨在提供最大的灵活性，同时保持代码的安全性和可读性。深入理解这一系统是成为一名高效Rust开发者的重要步骤。
