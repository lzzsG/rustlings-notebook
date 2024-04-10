# Exercise 41

- Name: ```modules1```
- Path: ```exercises/modules/modules1.rs```
#### Hint: 

Everything is private in Rust by default-- but there's a keyword we can use
to make something public! The compiler error should point to the thing that
needs to be public.


---



```rust,editable
// modules1.rs
//
// Execute `rustlings hint modules1` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

mod sausage_factory {
    // Don't let anybody outside of this module see this!
    fn get_secret_recipe() -> String {
        String::from("Ginger")
    }

    fn make_sausage() {
        get_secret_recipe();
        println!("sausage!");
    }
}

fn main() {
    sausage_factory::make_sausage();
}

```

---

### 本题内容：

这个练习要求你处理Rust模块中的私有性和公开性。Rust默认情况下所有的项（包括函数、结构体、枚举等）都是私有的，只能在其定义的当前模块内部访问。要让模块外部的代码能够访问某个项，你需要使用`pub`关键字来声明它为公开的。这个练习的目的是让你理解如何控制模块中项的可见性。

### 相关知识点：

- **模块系统**：Rust通过模块系统（modules）来组织代码，模块是项的集合，可以包含函数、结构体、枚举等。

- **私有性和公开性**：默认情况下，模块中的项是私有的，只能在定义它们的模块内部访问。使用`pub`关键字可以将项声明为公开的，从而允许外部代码访问。

- **`pub`关键字**：通过在模块中的项（如函数、结构体字段等）前面加上`pub`关键字，可以使它们在模块外部可见。

### 解题方法：

#### 修改`make_sausage`函数的可见性
- **步骤描述**：
  1. 在`make_sausage`函数声明前添加`pub`关键字，使得这个函数在模块外部可访问。

### 代码示例：

```rust
mod sausage_factory {
    // get_secret_recipe函数保持私有不变
    fn get_secret_recipe() -> String {
        String::from("Ginger")
    }

    // 使用pub关键字公开make_sausage函数
    pub fn make_sausage() {
        get_secret_recipe();
        println!("sausage!");
    }
}

fn main() {
    sausage_factory::make_sausage();
}
```

通过完成这个练习，你将学会如何控制Rust模块中项的可见性，了解如何通过`pub`关键字将模块中的函数等项公开，以及这种公开性对模块外部代码的影响。掌握模块和可见性是组织大型Rust项目和库的关键，它有助于封装和模块化代码，提高代码的重用性和维护性。

## 扩展知识点与解答：

在完成`modules1`练习之后，你现在应该对Rust中模块的基本概念和如何控制项的可见性有了一定的理解。模块是Rust中组织和封装代码的基本单位，通过模块，你可以将代码分组到不同的命名空间中，从而提高代码的可读性和重用性。以下是与本题相关的一些扩展知识点和解题方法，这些内容将帮助你更加深入地理解Rust的模块系统。

### 扩展知识点：

1. **模块定义和嵌套**：
   - Rust允许在一个文件中定义多个模块，也允许模块嵌套。通过模块嵌套，你可以创建清晰的模块结构，更好地组织你的代码。

2. **使用`mod.rs`和子模块**：
   - 当模块体积增大或者需要进一步组织时，可以将模块分割到不同的文件或目录中。通常，模块的子模块放在以模块名命名的目录下，其中`mod.rs`文件代表模块的根。

3. **路径和作用域**：
   - 在Rust中，模块还定义了项的路径。你可以使用绝对路径或相对路径来访问模块中的项。理解路径和作用域对于管理和访问模块中的项非常重要。

### 扩展解题方法：

1. **重构到多文件模块**：
   - 尝试将你的模块分割到不同的文件中。例如，可以将`sausage_factory`模块移到它自己的文件中，然后在主文件中通过`mod sausage_factory;`来引入它。

2. **探索公开性的细节**：
   - 除了函数，尝试将结构体、枚举和它们的字段公开。注意，即使一个结构体是公开的，其字段也不会自动公开。你需要显式地为每个字段添加`pub`关键字。

3. **使用`pub use`重导出项**：
   - Rust允许使用`pub use`语句重导出项，这样其他模块就可以直接访问这些项，而不需要知道它们的具体路径。这对于创建公共API接口特别有用。

#### 示例代码：

将模块分割到不同的文件：

```rust
// sausage_factory.rs
pub fn make_sausage() {
    // 函数体保持不变
}

// main.rs
mod sausage_factory;

fn main() {
    sausage_factory::make_sausage();
}
```

通过这些扩展内容，你将能够更加灵活地使用Rust的模块系统来组织你的代码。理解如何有效地分割和组织模块是构建大型Rust应用和库的关键。这不仅有助于代码的封装和重用，也使得项目更易于维护和扩展。
