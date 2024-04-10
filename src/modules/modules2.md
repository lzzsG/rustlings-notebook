# Exercise 42

- Name: ```modules2```
- Path: ```exercises/modules/modules2.rs```
#### Hint: 

The delicious_snacks module is trying to present an external interface that is
different than its internal structure (the `fruits` and `veggies` modules and
associated constants). Complete the `use` statements to fit the uses in main and
find the one keyword missing for both constants.

Learn more at [https://doc.rust-lang.org/book/ch07-04](https://doc.rust-lang.org/book/ch07-04-bringing-paths-into-scope-with-the-use-keyword.html#re-exporting-names-with-pub-use) ([Chinese version](https://rustwiki.org/zh-CN/book/ch07-04-bringing-paths-into-scope-with-the-use-keyword.html#%E4%BD%BF%E7%94%A8-pub-use-%E9%87%8D%E5%AF%BC%E5%87%BA%E5%90%8D%E7%A7%B0))


---



```rust,editable
// modules2.rs
//
// You can bring module paths into scopes and provide new names for them with
// the 'use' and 'as' keywords. Fix these 'use' statements to make the code
// compile.
//
// Execute `rustlings hint modules2` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

mod delicious_snacks {
    // TODO: Fix these use statements
    use self::fruits::PEAR as ???
    use self::veggies::CUCUMBER as ???

    mod fruits {
        pub const PEAR: &'static str = "Pear";
        pub const APPLE: &'static str = "Apple";
    }

    mod veggies {
        pub const CUCUMBER: &'static str = "Cucumber";
        pub const CARROT: &'static str = "Carrot";
    }
}

fn main() {
    println!(
        "favorite snacks: {} and {}",
        delicious_snacks::fruit,
        delicious_snacks::veggie
    );
}

```

---

### 本题内容：

这个练习要求你解决模块导入（`use`）的问题，并了解如何通过`pub use`重新导出项以构建模块的外部接口。你需要修复`delicious_snacks`模块中的`use`声明，以便在`main`函数中正确引用模块中的常量，并确保这些常量在模块外部是可见的。

### 相关知识点：

- **模块导入**：使用`use`关键字可以将模块、函数、结构体等导入当前作用域，简化其后的代码引用。

- **重命名导入项**：`use`语句可以结合`as`关键字重命名导入项，提供更适合当前上下文的名称。

- **重新导出**：使用`pub use`可以重新导出模块中的项，使得这些项可以被模块外部访问。这对于创建公共API和隐藏内部结构非常有用。

### 解题方法：

#### 修复`use`声明并重新导出常量
- **步骤描述**：
  1. 在`delicious_snacks`模块内，使用`pub use self::fruits::PEAR as fruit;`来导入并重命名`fruits`模块中的`PEAR`常量为`fruit`，同时将其重新导出。
  2. 同样地，使用`pub use self::veggies::CUCUMBER as veggie;`来导入并重命名`veggies`模块中的`CUCUMBER`常量为`veggie`，并重新导出。

### 代码示例：

```rust
mod delicious_snacks {
    // 修复这些use声明并重新导出常量
    pub use self::fruits::PEAR as fruit;
    pub use self::veggies::CUCUMBER as veggie;

    mod fruits {
        pub const PEAR: &'static str = "Pear";
        pub const APPLE: &'static str = "Apple";
    }

    mod veggies {
        pub const CUCUMBER: &'static str = "Cucumber";
        pub const CARROT: &'static str = "Carrot";
    }
}

fn main() {
    println!(
        "favorite snacks: {} and {}",
        delicious_snacks::fruit,
        delicious_snacks::veggie
    );
}
```

通过完成这个练习，你将学会如何在Rust中使用`use`、`as`和`pub use`关键字来导入、重命名和重新导出模块中的项。这些技能对于构建清晰、可维护的模块结构非常重要，它们允许你灵活地控制模块的内部和外部接口，从而创建出既简洁又强大的Rust应用程序和库。

## 扩展知识点与解答：

在深入`modules2`练习之后，你已经掌握了使用`use`、`as`、以及`pub use`进行模块项导入和重新导出的基本知识。这些是管理Rust模块及其公共接口的关键工具。以下是与本题相关的一些扩展知识点和解题方法，这些内容将帮助你更全面地理解Rust模块系统的能力和如何有效地使用它。

### 扩展知识点：

1. **可被导出的项**：几乎所有在模块中定义的项都可以被导出，包括结构体、枚举、常量、类型别名、函数、以及嵌套模块等。使用`pub`关键字声明这些项为公开，使得它们可以被模块外部访问。

2. **路径简化**：`use`声明不仅可以将项导入当前作用域，还可以通过创建路径的别名来简化复杂的路径。例如，通过`use crate::a::b::c::Module as MyModule;`可以简化对`Module`的引用。

3. **模块的组织结构**：理解如何将代码分割进不同的模块和子模块对于维护大型Rust项目是非常重要的。合理的模块结构可以增强代码的可读性和重用性。

4. **`pub(crate)`和`pub(super)`**：Rust提供了更细粒度的可见性控制。`pub(crate)`使项在当前crate内公开，而`pub(super)`使项在父模块内公开。

### 扩展解题方法：

1. **探索不同的导出策略**：尝试使用`pub(crate)`和`pub(super)`对模块项进行更细粒度的可见性控制，理解它们与简单的`pub`在实际项目中的不同应用场景。

2. **组织和重构模块**：随着项目的增长，考虑重构代码到不同的模块或子模块中。尝试将相关的功能组织到一起，使用模块来逻辑上分隔代码。

3. **深入嵌套模块的导入和导出**：当模块嵌套层数较多时，学习如何有效地使用`use`语句来简化路径，并通过`pub use`重新导出嵌套模块中的项，为外部使用者提供清晰的API。

#### 示例代码：

```rust
// 在crate的顶级定义模块
pub mod food {
    pub mod fruits {
        pub const APPLE: &'static str = "Apple";
    }

    pub mod veggies {
        pub const CARROT: &'static str = "Carrot";
    }
}

// 在crate的根文件或其他地方导入和使用
use crate::food::fruits::APPLE;
use crate::food::veggies::CARROT as MyFavoriteVeggie;

fn eat() {
    println!("Eating {} and {}", APPLE, MyFavoriteVeggie);
}
```

通过扩展你对Rust模块系统的理解，你将能够构建出结构清晰、易于维护的Rust应用程序和库。Rust的模块系统是它强大功能的基石之一，掌握如何有效地使用它对于成为一个高效的Rust开发者至关重要。
