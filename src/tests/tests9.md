# Exercise 100

- Name: ```tests9```
- Path: ```exercises/tests/tests9.rs```
#### Hint: 

No hints this time!


---



```rust,editable
// tests9.rs
//
// Rust is highly capable of sharing FFI interfaces with C/C++ and other 
// statically compiled languages, and it can even link within the code itself!
// It makes it through the extern block, just like the code below.
//
// The short string after the `extern` keyword indicates which ABI the 
// externally imported function would follow. In this exercise, "Rust" 
// is used, while other variants exists like "C" for standard C ABI, "stdcall"
// for the Windows ABI.
//
// The externally imported functions are declared in the extern blocks, with a
// semicolon to mark the end of signature instead of curly braces. Some 
// attributes can be applied to those function declarations to modify the
// linking behavior, such as #[link_name = ".."] to modify the actual symbol
// names.
//
// If you want to export your symbol to the linking environment, the `extern`
// keyword can also be marked before a function definition with the same ABI
// string note. The default ABI for Rust functions is literally "Rust", so if
// you want to link against pure Rust functions, the whole extern term can be
// omitted.
//
// Rust mangles symbols by default, just like C++ does. To suppress this
// behavior and make those functions addressable by name, the attribute
// #[no_mangle] can be applied.
//
// In this exercise, your task is to make the testcase able to call the 
// `my_demo_function` in module Foo. the `my_demo_function_alias` is an alias
// for `my_demo_function`, so the two
// line of code in the testcase should call the same function.
//
// You should NOT modify any existing code except for adding two lines of
// attributes.

// I AM NOT DONE

extern "Rust" {
    fn my_demo_function(a: u32) -> u32;
    fn my_demo_function_alias(a: u32) -> u32;
}

mod Foo {
    // No `extern` equals `extern "Rust"`.
    fn my_demo_function(a: u32) -> u32 {
        a
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_success() {
        // The externally imported functions are UNSAFE by default
        // because of untrusted source of other languages. You may
        // wrap them in safe Rust APIs to ease the burden of callers.
        //
        // SAFETY: We know those functions are aliases of a safe
        // Rust function.
        unsafe {
            my_demo_function(123);
            my_demo_function_alias(456);
        }
    }
}

```

---

### 本题内容

本题的目标是让学生理解和实践 Rust 的外部函数接口（FFI）特性，特别是如何通过 `extern` 声明来链接和调用其他编程语言（如 C 或 C++）中的函数。此外，还要掌握如何通过 Rust 函数定义与外部环境交互。

### 相关知识点

1. **Extern 声明**:
   - `extern` 关键字用于声明外部函数，表明这些函数的实现在 Rust 代码之外。
   - `"Rust"` ABI（应用程序二进制接口）表示这些函数遵循 Rust 的调用约定，常用于 Rust 间的模块调用。

2. **ABI 的使用**:
   - ABI 定义了函数参数如何传递以及返回值如何处理。
   - 除了 `"Rust"`，常见的 ABI 包括 `"C"`（用于与 C 语言互操作）和 `"stdcall"`（主要在 Windows 上使用）。

3. **#[no_mangle] 属性**:
   - 通常 Rust 会改变函数名（名字修饰）以支持多态和命名空间，使用 `#[no_mangle]` 可以禁止这种行为，使得函数名在编译后不会被改变，常用于与其他语言的互操作中。

4. **安全性考量**:
   - 使用 `extern` 声明的函数默认为 `unsafe`，因为 Rust 不能保证外部代码的安全性。即使是 Rust 代码，也需明确标记为安全的上下文中调用。

### 解题方法

- **修正外部函数声明**:
  - 确保 `extern` 声明中的函数与 `Foo` 模块中的函数签名一致。
  - 使用 `#[no_mangle]` 确保 Rust 不会改变函数名。

- **添加属性**:
  - 为 `my_demo_function` 和 `my_demo_function_alias` 添加正确的链接属性，使其能够正确地链接到 `Foo` 模块中定义的函数。

- **安全调用**:
  - 在测试中，通过 `unsafe` 块安全地调用这些外部函数，注明为什么这样做是安全的。

### 代码示例

修改 `Foo` 模块中的函数定义，确保它们可以被外部访问：

```rust
mod Foo {
    #[no_mangle]  // 确保函数名不被修饰，使得外部链接可以找到它
    pub extern "Rust" fn my_demo_function(a: u32) -> u32 {
        a
    }
}
```

在 `extern` 块中添加对应的外部链接声明：

```rust
extern "Rust" {
    fn my_demo_function(a: u32) -> u32;
    fn my_demo_function_alias(a: u32) -> u32;
}
```

通过上述修改，你可以确保函数在不同模块间正确链接，并且在测试中能够安全地调用它们。


---

## 扩展知识点与解答：

### 扩展知识点

1. **Foreign Function Interface (FFI)**:
   - Rust 提供了一种与其他语言交互的方式，称为 Foreign Function Interface (FFI)。通过 FFI，Rust 程序可以调用 C、C++、Fortran 等语言编写的函数，同时也可以暴露 Rust 函数给这些语言调用。
   - FFI 的使用不仅限于调用外部函数，也涉及到数据类型转换和内存管理，因为不同语言的数据布局和内存管理策略可能不同。

2. **Symbol Mangling**:
   - Symbol mangling 是一种编译器技术，用于生成独特的名字，以区分具有相同名字但签名不同的函数或变量。
   - 在使用 FFI 时，通常需要禁用 symbol mangling，确保函数名在编译后的二进制文件中保持不变，使得不同的编程语言可以正确链接和调用这些函数。

3. **ABI Compatibility**:
   - 应用二进制接口（ABI）定义了不同编译单元之间如何调用函数，包括参数如何传递、如何布局以及如何返回。
   - 在跨语言链接中正确设置 ABI 是非常重要的，因为不同语言或不同编译器可能使用不同的 ABI。

### 扩展解题方法

1. **使用 `extern` 关键字定义外部函数**:
   - 使用 `extern` 关键字加上 ABI 名来声明外部函数。例如 `extern "C"` 声明遵循 C 语言的调用约定。如果是 Rust 间的调用，默认的 ABI 是 `"Rust"`。

2. **在 Rust 中导出函数**:
   - 使用 `#[no_mangle]` 属性来禁用 Rust 的名称修饰，以确保函数名不被改变。
   - 函数需要被声明为 `pub` 以确保它们可以被外部模块访问。
   - 使用 `extern` 关键字标记函数，指定它们应遵循的 ABI。

3. **调试链接问题**:
   - 当遇到函数无法正确链接的问题时，检查函数的声明和定义是否一致，包括函数签名和 ABI。
   - 确保没有名称修饰问题，特别是在涉及多种编译器或需要与其他语言互操作时。
   - 使用工具如 `nm` 或 `objdump` 检查编译后的二进制文件，验证符号是否存在。

### 背后原理

**外部函数接口 (FFI) 和 `extern` 关键字**:

1. **FFI 的工作原理**:
   - FFI 允许 Rust 代码直接与编译成机器码的其他语言的库或函数进行交互。这是通过在 Rust 中声明外部函数原型来实现的，这些函数实现在其他语言中。
   - 在调用这些外部函数时，Rust 编译器需要知道函数的调用约定（如参数如何传递、如何布局以及如何返回），这是通过 ABI (应用二进制接口) 来规定的。

2. **使用 `extern` 声明外部函数**:
   - `extern` 关键字用于声明那些在 Rust 代码之外实现的函数。这通常涉及非 Rust 库（如 C 库）。
   - `extern` 后面可以跟 ABI 名称，如 `extern "C"` 表示函数遵循 C 语言的标准调用约定。默认情况下，如果没有指定 ABI，`extern` 块假定为 C ABI。

3. **符号修饰 (Mangling) 和 `#[no_mangle]` 属性**:
   - 为了支持重载和特殊的类型信息，Rust 和 C++ 这样的语言会修改函数和变量名（称为符号修饰或 mangling）。这使得链接阶段可以区分同名但签名不同的函数。
   - 使用 `#[no_mangle]` 属性可以禁止 Rust 对特定函数进行符号修饰，确保函数名在编译后的二进制文件中不变，从而可以被其他语言或 ABI 准确地引用。

### 常用场景

**1. 调用 C/C++ 库**:
   - Rust 项目中经常需要使用现有的 C 或 C++ 库，特别是在系统编程、游戏开发、嵌入式系统等领域。通过 FFI，可以直接使用这些库中提供的丰富功能，而无需用 Rust 重写。【[关于裸机编程](https://lzzs.fun/zh/blog/bare-metal/0-About-bare-metal-programming)】

**2. 提供 Rust 库给其他语言使用**:

   - Rust 以内存安全著称，因此可能希望用 Rust 实现一些关键功能，然后在 Python、Ruby 或其他语言中调用这些功能。通过 `extern "C"` 和 `#[no_mangle]`，Rust 函数可以被其他语言安全调用。

**3. 性能关键型代码**:
   - 在某些性能敏感的应用中，可能需要确保使用最有效的方式调用函数，特别是在低延迟或高吞吐量的系统中。通过精确控制 ABI 和通过 FFI 与汇编代码或其他高性能库交互，可以达到这一目的。

通过正确使用 `extern`、ABI、和 `#[no_mangle]` 等工具，Rust 程序员可以有效地扩展其应用的能力，同时保持代码的性能和安全性。
