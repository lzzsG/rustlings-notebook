# Exercise 84

- Name: ```macros1```
- Path: ```exercises/macros/macros1.rs```
#### Hint: 

When you call a macro, you need to add something special compared to a regular function call. If you're stuck, take a look at what's inside `my_macro`.


---



```rust,editable
// macros1.rs
//
// Execute `rustlings hint macros1` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

macro_rules! my_macro {
    () => {
        println!("Check out my macro!");
    };
}

fn main() {
    my_macro();
}

```

---

### 本题内容

本练习的目标是引导学习者了解和使用 Rust 中的宏（macros）。宏是 Rust 中一种强大的元编程工具，允许在编译时执行代码。宏可以用来执行重复的任务或编写可扩展的功能。

### 相关知识点

1. **宏与函数的区别**：
   - 宏在编译时展开，而函数是在运行时调用。
   - 宏可以接受可变数量的参数，包括不同类型的参数。
   - 宏的调用需要特殊的语法，通常是使用 `!`。

2. **宏的定义**：
   - 使用 `macro_rules!` 来定义宏。
   - 宏的体内可以包含模式和相应的代码块，支持匹配不同形式的调用。

3. **宏的使用**：
   - 宏调用需要在宏名后加上感叹号 `!`，表示这是宏的调用而非函数调用。

### 解题方法

#### 步骤描述

1. **理解宏的定义**：
   - 查看 `my_macro` 的定义，理解其语法结构和功能。

2. **修正宏调用**：
   - 根据宏的定义，正确调用 `my_macro`，确保它可以在编译时被正确展开。

3. **编译和测试**：
   - 编译代码，确保没有错误。
   - 运行程序，检查输出是否符合预期。

#### 代码示例

正确调用宏的代码应该如下：

```rust
// macros1.rs
// 已完成宏定义和调用

macro_rules! my_macro {
    () => {
        println!("Check out my macro!");
    };
}

fn main() {
    my_macro!();  // 正确的宏调用，注意感叹号！
}
```

通过这个练习，你将学习到宏的基础使用方法，包括如何定义和调用宏。掌握宏的使用对于编写高效、可复用且易于维护的 Rust 代码非常重要。宏在处理复杂的编译时代码生成中尤其有用，能够帮助简化代码结构并提高代码的灵活性。

## 扩展知识点与解答

### 扩展知识点

1. **宏的工作原理**：
   宏在 Rust 的编译阶段进行处理，它们是所谓的“语法抽象”，允许写出能够接收并操作 Rust 语法树的代码。这使得宏非常强大，因为它们可以在编译时生成和转换代码，从而实现复杂的模式和重复逻辑的抽象。

2. **宏的类型**：
   - **声明宏**（Declarative Macros）：使用 `macro_rules!` 创建，如本练习中的示例。它们类似于模式匹配，根据所提供的规则展开。
   - **过程宏**（Procedural Macros）：更复杂的宏类型，包括自定义 `#[derive]` 宏、属性类宏和函数类宏。这些宏作用于 Rust 代码的结构，可以创建更强大的抽象。

3. **宏的用例**：
   宏广泛用于代码生成、条件编译、简化重复的代码结构等场景。例如，通过宏可以轻松地定义新的数据结构或实现特定的特征方法。

### 扩展解题方法

1. **宏的调试**：
   - 宏的调试通常比函数更复杂，因为错误可能发生在宏展开的任何地方。使用 `cargo expand` 命令（需要安装 `cargo-expand` 包）可以帮助查看宏展开后的代码，这对理解宏的行为非常有帮助。

2. **宏的设计模式**：
   - 学习和应用有效的宏设计模式是提高宏效率的关键。例如，尽量避免宏内的重复代码，使用嵌套宏来处理复杂的输入，以及使用递归宏来处理可变长度的输入。

3. **宏的优化**：
   - 宏虽然强大，但不恰当的使用会导致代码膨胀和编译时间增长。优化宏的关键在于合理使用，只在无法通过函数或其他手段简洁表达逻辑时采用宏。

4. **实用宏示例**：
   - 学习常见库中宏的使用，如 `serde` 中的序列化/反序列化宏 `serde_derive`，或是 `log` 库中用于日志记录的宏。这些实例可以提供如何在实际项目中有效使用宏的灵感。

通过这些扩展知识点和解题方法，你可以更深入地理解和运用 Rust 中的宏，不仅限于基本的声明宏，还包括更复杂的过程宏和它们在实际应用中的用途。这将极大地扩展你使用 Rust 进行高效编程的能力。

## 声明宏（Declarative Macros）

声明宏（Declarative Macros），也常被称为“宏规则”（macro_rules!），是 Rust 中一种强大的代码生成工具，允许在编译时根据给定的规则进行模式匹配和代码扩展。这种宏是用来创建可以重复使用的代码模板的，非常适合用于减少重复代码、进行条件编译、或者构建复杂的数据结构和API。

### 基本结构

声明宏通过 `macro_rules!` 宏进行定义。其基本结构包括：
- **宏名称**：定义宏的名称。
- **一组规则**：每个规则由一个模式和相应的扩展代码组成。模式用于匹配宏输入，扩展代码定义了宏在匹配成功时应该如何展开。

### 语法和特性

```rust
macro_rules! my_macro {
    // 规则 1
    (pattern) => {
        expansion;
    };
    // 规则 2
    (pattern) => {
        expansion;
    };
}
```

- **模式**（Pattern）：可以包括各种宏设计符（designators），如：
  - `expr`：表达式。
  - `tt`（token tree）：标记树，匹配几乎任何Rust代码。
  - `ident`：标识符，用于变量名或函数名。
  - `path`：路径，例如 `std::result::Result`。
  - `ty`：类型（Type）。
  - `$x:expr` 表示一个表达式匹配，其中 `$x` 是一个变量，`expr` 是其类型。

- **展开**（Expansion）：定义了当模式匹配成功时，宏应该如何扩展。它可以包含任意的 Rust 代码，包括其他宏的调用。

### 实例

```rust
macro_rules! say_hello {
    () => {
        println!("Hello!");
    }
}

fn main() {
    say_hello!();
}
```

这个简单的宏在调用时会展开为打印“Hello!”的代码。

### 高级用途

1. **重载**：声明宏支持重载，即可以根据输入参数的不同模式来执行不同的代码。
   
```rust
macro_rules! calc {
    (add $a:expr, $b:expr) => {{
        $a + $b
    }};
    (sub $a:expr, $b:expr) => {{
        $a - $b
    }};
}

fn main() {
    let sum = calc!(add 5, 7);
    let difference = calc!(sub 10, 4);
    println!("Sum: {}, Difference: {}", sum, difference);
}
```

2. **递归宏**：宏可以递归调用自身，用于生成复杂或可变长度的数据结构。

声明宏是 Rust 中一种极为强大的工具，它通过在编译时进行代码生成，不仅可以减少重复代码，还可以用于构建抽象层和API，同时还支持高度的定制化和灵活性。正确理解和使用声明宏可以极大地提高 Rust 程序的表达力和效率。

### 常见的宏设计符

更多内容参见[（Rust 宏小册）](https://zjp-cn.github.io/tlborm/introduction.html)

在 Rust 中，声明宏通过模式来匹配输入，并根据这些输入展开相应的代码。每个模式都关联着一个或多个设计符（designator），用于捕获特定类型的宏输入。以下是一些更广泛的模式示例，展示了宏如何通过不同的设计符捕获和处理各种类型的输入：

1. **`expr`**：匹配一个表达式。
2. **`ident`**：匹配一个标识符，常用于变量名或函数名。
3. **`path`**：匹配一个路径，例如 `std::vec::Vec`。
4. **`ty`**：匹配一个类型。
5. **`block`**：匹配一个代码块。
6. **`stmt`**：匹配一个语句。
7. **`pat`**：匹配一个模式（通常用在模式匹配中）。
8. **`item`**：匹配一个项，如函数、结构体、枚举等。
9. **`meta`**：匹配元数据（属性）。
10. **`tt`**（token tree）：匹配单个标记或标记树。

### 实例展示

这里展示几个使用不同模式匹配的宏示例，以体现它们的应用多样性：

#### 匹配标识符和表达式

```rust
macro_rules! create_function {
    ($func_name:ident) => {
        fn $func_name() {
            println!("You called {:?}()", stringify!($func_name));
        }
    };
}

create_function!(foo);
create_function!(bar);

fn main() {
    foo();
    bar();
}
```

这个宏创建了具有指定名称的函数，`$func_name:ident` 匹配一个标识符。

#### 匹配路径和类型

```rust
macro_rules! add_impl {
    ($path:path, $type:ty) => {
        impl $path for $type {
            fn add(self, other: Self) -> Self {
                self + other
            }
        }
    };
}

add_impl!(std::ops::Add, i32);

fn main() {
    let result = 10.add(20);
    println!("Result: {}", result);
}
```

这个宏为指定的类型实现了 `std::ops::Add` trait。

#### 使用 token tree

```rust
macro_rules! count_args {
    () => { 0usize };
    ($head:tt $($tail:tt)*) => { 1usize + count_args!($($tail)*) };
}

fn main() {
    println!("Number of args: {}", count_args!(1, "two", 3, 4));
}
```

这个宏使用 `tt` 设计符来计算传递给宏的参数数量。`$head:tt` 和 `$($tail:tt)*` 模式用来递归地计数。

通过上述示例，可以看出宏设计符在 Rust 中的强大灵活性，允许宏以几乎无限的方式操作和生成代码。掌握这些模式匹配技巧是高效使用 Rust 宏的关键。

## 过程宏（Procedural Macros）

过程宏（Procedural Macros）在Rust中提供了一种更为强大和灵活的方式来操作和生成代码。与声明宏（`macro_rules!`）不同，过程宏作用于Rust代码的语法树（Abstract Syntax Tree，AST），允许开发者编写代码来操作这些结构。这种宏更像是小型的编译器插件。

### 过程宏的类型

过程宏主要有三种类型：

1. **属性宏（Attribute macros）**：
   - 类似于语言内置的属性（如 `#[derive]`），可用于结构体、函数、模块等。
   - 可以创建新的属性，或者修改通过它们标记的项的行为。

2. **派生宏（Derive macros）**：
   - 允许自定义 `derive` 属性的行为，自动为类型实现特定的特性。
   - 例如，可以自定义派生 `Debug` 或 `Clone` 等特性的实现方式。

3. **函数宏（Function-like macros）**：
   - 看起来和调用函数类似，但它们操作的是传递给它们的令牌。
   - 可以接受复杂的输入，并生成任意的代码。

### 1. 属性宏（Attribute macros）

属性宏（Attribute macros）能够附加到几乎任何类型的 Rust 项上，从而允许修改其定义或者生成额外的代码。下面的例子将演示一个更复杂的属性宏，该宏能够记录函数的执行时间，并在函数执行前后打印日志消息。

#### 定义属性宏

我们将创建一个名为 `trace_execution` 的属性宏，该宏可以被附加到函数上，用于记录函数的执行时间和打印执行前后的日志：

```rust
extern crate proc_macro;
use proc_macro::TokenStream;
use quote::{quote, format_ident};
use syn::{parse_macro_input, ItemFn};

#[proc_macro_attribute]
pub fn trace_execution(_attr: TokenStream, item: TokenStream) -> TokenStream {
    let input_fn = parse_macro_input!(item as ItemFn); // 解析输入的函数
    let fn_name = &input_fn.sig.ident; // 获取函数名
    let fn_block = &input_fn.block; // 获取函数体
    let inputs = &input_fn.sig.inputs; // 获取函数输入参数
    let output = &input_fn.sig.output; // 获取函数返回类型

    let log_start = format_ident!("start_{}", fn_name);
    let log_end = format_ident!("end_{}", fn_name);

    let expanded = quote! {
        fn #fn_name(#inputs) #output {
            let #log_start = std::time::Instant::now();
            println!("Starting: {}", stringify!(#fn_name));

            let result = (|| #fn_block)(); // 执行原始函数体

            let #log_end = #log_start.elapsed();
            println!("Completed: {} in {:?}", stringify!(#fn_name), #log_end);

            result
        }
    };

    TokenStream::from(expanded)
}
```

#### 使用属性宏

现在，我们可以将这个宏应用到任何函数上，以自动记录其执行时间和生成日志：

```rust
#[trace_execution]
fn compute(data: &str) -> usize {
    println!("Processing data: {}", data);
    // 模拟一些计算
    thread::sleep(std::time::Duration::from_secs(1));
    data.len()
}

fn main() {
    let result = compute("Hello, world!");
    println!("Result of computation: {}", result);
}
```

#### 运行和结果

当你运行上述代码时，将观察到以下输出：
- 打印出函数开始执行的日志。
- 执行函数体。
- 打印出函数执行完成的日志，包括执行耗时。

这个属性宏示例展示了如何在编译时插入对函数执行进行监控的代码，不仅增加了函数的执行日志，还包括了执行时间的统计。这种方式对于开发高性能应用和进行性能分析非常有帮助，使得开发者可以在不修改原始代码的情况下，快速地为程序的关键部分添加性能监控和日志记录功能。

### 2. 派生宏（Derive macros）

派生宏（Derive macros）使得开发者可以自定义类型的自动派生行为，例如为自定义类型实现`Debug`、`Clone`等常见的Rust trait。以下是一个简单的派生宏示例，演示如何为一个自定义类型自动实现一个简单的`Description` trait。

首先，我们定义一个`Description` trait，它包含一个返回描述字符串的方法：

```rust
pub trait Description {
    fn describe(&self) -> String;
}
```

接下来，我们创建一个派生宏，自动为标记了`#[derive(Describe)]`的结构体生成`Description` trait的实现。这个宏将使用`syn`库来解析输入的Rust代码，并使用`quote`库来生成输出的Rust代码：

```rust
extern crate proc_macro;
use proc_macro::TokenStream;
use quote::quote;
use syn::{parse_macro_input, DeriveInput};

#[proc_macro_derive(Describe)]
pub fn derive_description(input: TokenStream) -> TokenStream {
    let input = parse_macro_input!(input as DeriveInput); // 解析输入的函数
    let name = &input.ident;  // 获取函数名

    let expanded = quote! {
        impl Description for #name {
            fn describe(&self) -> String {
                format!("This is an instance of {}.", stringify!(#name))
            }
        }
    };

    TokenStream::from(expanded)
}
```

这个派生宏`derive_description`将为任何使用`#[derive(Describe)]`标记的Rust结构体实现`Description` trait。生成的方法`describe`将返回一个描述该结构体的字符串。

#### 使用示例

现在，我们可以使用这个派生宏来为任意结构体自动实现`Description` trait：

```rust
#[derive(Describe)]
struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let point = Point { x: 10, y: 20 };
    println!("{}", point.describe());  // 输出: "This is an instance of Point."
}
```

在这个例子中，`Point` 结构体通过`#[derive(Describe)]`自动获得了`Description` trait的实现。当我们创建`Point`的实例并调用`describe`方法时，它将输出预定义的字符串，表明这是一个`Point`的实例。

派生宏为Rust开发者提供了极大的便利，特别是在需要为多种类型提供统一行为时。通过使用派生宏，可以显著减少重复的代码量，同时增强代码的一致性和可维护性。

### 3. 函数宏（Function-like macros）

函数宏（Function-like macros）类似于函数调用，它们可以在被调用的位置插入生成的代码。这种类型的过程宏不仅允许我们生成简单的代码，还可以处理复杂的输入和产生高度定制的输出。接下来，我将展示一个复杂的函数宏示例，它处理输入参数并根据这些参数生成 Rust 代码。

#### 示例：创建一个条件日志宏

我们将创建一个名为 `conditional_log!` 的函数宏，该宏接受一个日志等级和消息，根据当前的日志等级设置（模拟）来决定是否输出消息。这个宏将展示如何使用条件语句和变量处理来生成代码。

```rust
extern crate proc_macro;
use proc_macro::TokenStream;
use quote::quote;
use syn::parse_macro_input;
use syn::parse::{Parse, ParseStream};
use syn::{LitStr, Token};

// 自定义输入解析结构
struct LogInput {
    level: LitStr,
    message: LitStr,
}

impl Parse for LogInput {
    fn parse(input: ParseStream) -> syn::Result<Self> {
        let level: LitStr = input.parse()?;
        input.parse::<Token![,]>()?; // 解析并消耗逗号
        let message: LitStr = input.parse()?;
        Ok(LogInput { level, message })
    }
}

#[proc_macro]
pub fn conditional_log(input: TokenStream) -> TokenStream {
    let LogInput { level, message } = parse_macro_input!(input as LogInput);

    // 模拟的日志等级环境变量（在实际应用中，这里可以是环境变量或配置文件中的设置）
    let log_level_env = "INFO"; // 只有INFO级别及以上的日志会被打印

    // 根据日志等级生成不同的代码
    let expanded = match level.value().as_str() {
        "ERROR" | "WARN" | "INFO" => quote! {
            if #level == #log_level_env || #level == "ERROR" || #level == "WARN" {
                println!("[{}] {}", #level, #message);
            }
        },
        _ => quote! {
            if #level == #log_level_env {
                println!("[{}] {}", #level, #message);
            }
        },
    };

    TokenStream::from(expanded)
}

```

#### 使用宏

```rust
// 在 Rust 代码中使用此宏
fn main() {
    conditional_log!("INFO", "This is an information message.");
    conditional_log!("DEBUG", "This debug message will not be printed.");
}
```

#### 宏功能说明

这个宏定义中，我们首先定义了一个解析结构 `LogInput`，用来解析和存储宏输入参数。它使用 `syn` 库的解析功能来处理输入的日志等级和消息文本。

在宏本身中，我们根据传入的日志等级和一个模拟的环境变量 `log_level_env` 来决定是否输出日志。这里的条件编译确保只有当日志等级匹配时才输出消息。

这个函数宏展示了如何在宏中使用条件逻辑和文本处理，以及如何处理稍微复杂的输入。这种宏在实际应用中非常有用，特别是在需要根据不同条件动态生成代码的场景中。

## 编译时代码生成

在 Rust 中，宏和普通函数在代码生成时机和机制上存在显著区别。理解这些区别对于有效使用 Rust 的功能至关重要，尤其是在涉及性能优化和代码抽象时。下面将详细探讨宏的编译时代码生成与普通函数的不同，以及它们在代码生成时机上的关键差异。

### 宏的编译时代码生成

宏在 Rust 中特别强大，因为它们在编译时就执行代码生成。这意味着宏在程序的源代码被编译成机器代码之前就已经展开，其生成的代码会成为编译过程的一部分。

**特点和时机**：
- **编译时展开**：宏在编译器处理源代码时展开，因此在编译器生成抽象语法树（AST）之前，宏就已经生成并插入了新的代码。这允许宏操作代码结构（如添加函数、变量定义等），并在编译器继续其余编译工作前影响最终的程序结构。
- **代码生成能力**：宏能够基于它们的输入生成各种代码结构，这包括条件编译代码块、重复代码结构等。由于是在编译时进行，这种生成的代码能直接受益于编译器的优化。

### 普通函数的运行时代码执行

与宏不同，普通函数在编译时不生成或修改代码，而是在程序运行时被调用和执行。

**特点和时机**：
- **运行时执行**：普通函数的代码在编译时被编译成机器代码，并在程序运行时按需执行。函数的执行不会改变程序的代码结构，只是在其自身的作用域内操作数据。
- **动态性**：普通函数可以根据运行时的数据做出决策（如通过参数传递），但它们不能根据这些数据动态生成新的程序代码。

### 代码生成时机的影响

这两种方法在何时生成代码上的区别带来了各自的优缺点：

- **优化和效率**：由于宏在编译时就已经展开，所以编译器可以优化宏生成的代码，就像它优化手写的代码一样。相反，普通函数的性能优化只限于其内部逻辑和调用效率，而不涉及结构性改变。
- **灵活性和维护性**：宏能在编译时根据提供的参数动态改变代码结构，这在生成重复代码或根据编译时已知的条件调整代码时非常有用。普通函数更容易编写和理解，对于那些不需要编译时决定的逻辑来说，它们提供了更好的维护性和灵活性。

理解 Rust 中宏的编译时代码生成与普通函数的运行时执行的区别，对于高效和正确地使用语言特性非常关键。宏提供了在编译时根据复杂规则生成代码的能力，而普通函数则提供了运行时的动态性和灵活性。根据应用场景选择正确的工具，是编写高效、可维护和高性能 Rust 程序的关键。
