# Exercise 96

- Name: ```tests5```
- Path: ```exercises/tests/tests5.rs```
#### Hint: 

For more information about `unsafe` and soundness, see
[https://doc.rust-lang.org/nomicon/safe-unsafe-meaning.html](https://doc.rust-lang.org/nomicon/safe-unsafe-meaning.html) ([Chinese version](https://nomicon.purewhite.io/safe-unsafe-meaning.html))




---



```rust,editable
// tests5.rs
//
// An `unsafe` in Rust serves as a contract.
//
// When `unsafe` is marked on an item declaration, such as a function,
// a trait or so on, it declares a contract alongside it. However,
// the content of the contract cannot be expressed only by a single keyword.
// Hence, its your responsibility to manually state it in the `# Safety`
// section of your documentation comment on the item.
//
// When `unsafe` is marked on a code block enclosed by curly braces,
// it declares an observance of some contract, such as the validity of some
// pointer parameter, the ownership of some memory address. However, like
// the text above, you still need to state how the contract is observed in
// the comment on the code block.
//
// NOTE: All the comments are for the readability and the maintainability of
// your code, while the Rust compiler hands its trust of soundness of your
// code to yourself! If you cannot prove the memory safety and soundness of
// your own code, take a step back and use safe code instead!
//
// Execute `rustlings hint tests5` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

/// # Safety
///
/// The `address` must contain a mutable reference to a valid `u32` value.
unsafe fn modify_by_address(address: usize) {
    // TODO: Fill your safety notice of the code block below to match your
    // code's behavior and the contract of this function. You may use the
    // comment of the test below as your format reference.
    unsafe {
        todo!("Your code goes here")
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_success() {
        let mut t: u32 = 0x12345678;
        // SAFETY: The address is guaranteed to be valid and contains
        // a unique reference to a `u32` local variable.
        unsafe { modify_by_address(&mut t as *mut u32 as usize) };
        assert!(t == 0xAABBCCDD);
    }
}

```

---

### 本题内容：

本练习旨在引导学生深入理解和实践 Rust 中的 `unsafe` 关键字的使用。通过这个练习，学生将学习如何在 Rust 中安全地使用 `unsafe` 代码块和函数，以及如何正确地文档化安全约定以保证代码的安全性和正确性。

### 相关知识点：

- **`unsafe` 关键字**：在 Rust 中，`unsafe` 关键字用于绕过 Rust 的安全检查。通常用于当自动的内存安全保证不能证明代码的安全性时，例如在处理裸指针或调用不安全的外部函数时。
- **安全文档**：使用 `unsafe` 代码时，必须在代码中加入适当的文档说明为何此段代码是安全的，这通常通过在函数上方使用文档注释来完成。
- **内存安全性**：`unsafe` 的使用需要保证所有的内存访问都是有效的，比如确保指针指向的是有效的内存，且访问没有违反任何借用规则。

### 解题方法：

1. **理解 `unsafe` 代码的约束**：首先，你需要理解为何需要使用 `unsafe`，通常是因为要执行 Rust 安全模型无法保证安全的操作。
2. **实现 `unsafe` 函数**：根据函数的定义和要求，实现 `modify_by_address` 函数，确保使用传入的地址修改数据时遵循内存安全规则。
3. **文档化安全约定**：在函数上方使用文档注释来详细说明为何这段代码是安全的，包括对于如何正确传入参数的描述。
4. **编写测试**：测试 `unsafe` 函数，确保在安全的约定下函数表现正常，并验证修改是否成功。

### 代码示例：

这里我们提供 `modify_by_address` 函数的一个可能的实现，以及相应的安全文档说明：

```rust
/// # Safety
///
/// The `address` must contain a mutable reference to a valid `u32` value.
/// The caller must ensure that the address points to a valid mutable `u32`.
unsafe fn modify_by_address(address: usize) {
    // SAFETY: As per the function contract, we assume `address` is a valid pointer
    // to a mutable `u32` and it's safe to dereference.
    let ptr = address as *mut u32;
    unsafe {
        // Dereferencing the raw pointer to modify the value.
        // It's safe to write because we ensure the pointer points to a valid `u32`.
        *ptr = 0xAABBCCDD;
    }
}
```

上述实现中，我们首先将 usize 类型的地址转换为 `*mut u32` 类型的裸指针，然后在另一个 `unsafe` 代码块中解引用并修改它指向的值。同时，我们在文档中详细说明了为何这段操作是安全的，即保证传入的地址是有效的。


---

## 扩展知识点与解答：

### 扩展知识点：

1. **`unsafe` 的深层理解**：在 Rust 中，`unsafe` 关键字提供了逃逸语言的严格安全保证的方法。了解何时以及如何安全地使用 `unsafe` 是高级 Rust 编程的重要部分。`unsafe` 用于那些编译器无法保证安全的场景，比如直接的内存操作、调用 C 语言接口等。

2. **裸指针**：裸指针（raw pointers）是 Rust 中的一种指针类型，分为 `*const T` 和 `*mut T`，分别代表不可变和可变裸指针。与引用不同，裸指针无需借用检查，因此它们的使用是不安全的。

3. **文档注释和安全协议**：在使用 `unsafe` 代码时，通常需要在文档注释中明确指出其安全性，这种做法有助于未来的代码维护和审查。文档注释应详细描述为何该段 `unsafe` 代码是安全的，以及使用它需要满足什么条件。

### 扩展解题方法：

1. **编写清晰的安全注释**：在实现 `unsafe` 代码时，应该编写清晰的安全注释来解释为什么此代码是安全的。注释应详细到足以让其他开发者理解假设的前提和为何这些假设使得代码安全。

2. **避免使用 `unsafe`**：在可能的情况下，尝试找到避免使用 `unsafe` 的方法。通常，Rust 的安全子集足以处理大多数情况。只有当性能要求极高或需要与低级系统接口直接交互时，才考虑使用 `unsafe`。

3. **测试和验证**：对于涉及 `unsafe` 的代码，进行彻底的测试尤其重要。这包括单元测试和可能的模糊测试，以确保在各种边缘情况下，`unsafe` 代码的行为符合预期。

## 关于unsafe

在 Rust 中，`unsafe` 关键字的使用是一个重要的主题，因为它允许开发者执行那些通常被 Rust 的借用检查器（borrow checker）所阻止的操作。这包括一些可能导致未定义行为（undefined behavior）的操作，因此使用 `unsafe` 需要格外小心。下面详细介绍 `unsafe` 的用途、格式和注意事项。

### 什么时候使用 `unsafe`

1. **解引用裸指针（Raw Pointers）**：
   - 裸指针 `*const T` 和 `*mut T` 可以在安全代码中创建和传递，但不能在不使用 `unsafe` 的情况下被解引用。

2. **调用不安全的函数或方法**：
   - 某些底层系统操作或外部函数调用（例如，从 C 语言继承的函数）可能不会保证 Rust 的安全保障，因此被标记为 `unsafe`。

3. **访问或修改可变静态变量**：
   - 可变静态变量可以被多个线程访问或修改而不受限制，可能导致数据竞争，因此访问它们被认为是不安全的。

4. **实现不安全的 trait**：
   - 如果 trait 本身定义了某些不保证内存安全的方法，则实现该 trait 必须使用 `unsafe`。

### `unsafe` 的格式和结构

- **不安全函数/方法**：
   ```rust
   unsafe fn do_something_unsafe() {
       // 执行不安全操作
   }
   ```

- **不安全块**：
   ```rust
   // 安全函数中的不安全操作
   fn some_function() {
       unsafe {
           // 执行不安全操作
       }
   }
   ```

### 安全准则和文档

使用 `unsafe` 代码时，关键是必须确保代码块内的所有操作都不会违反 Rust 的内存安全保证。这通常意味着需要遵守以下准则：

1. **保证裸指针的有效性**：
   - 确保解引用的裸指针指向有效的内存。

2. **避免数据竞争**：
   - 在并发上下文中使用裸指针或其他不安全代码时，确保适当的同步。

3. **文档化安全约定**：
   - 使用 `# Safety` 部分在文档注释中清楚地说明为什么这段 `unsafe` 代码是安全的，包括它依赖的所有假设。

### 例子和最佳实践

- **最小化 `unsafe` 代码的使用**：
   - 将 `unsafe` 代码封装在安全的抽象背后，并尽量减少暴露给外部的 `unsafe` 代码。

- **严格的代码审查**：
   - `unsafe` 代码应该经过严格的审查，确保其安全性无疑问。

- **测试**：
   - 尽管 `unsafe` 代码的行为可能不受标准 Rust 保障，但仍然可以并且应该进行彻底测试。

通过理解和遵守这些指导原则，你可以更安全地使用 Rust 的 `unsafe` 功能来执行底层系统级任务，同时保持应用的整体安全和可靠性。
