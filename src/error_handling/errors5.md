# Exercise 55

- Name: ```errors5```
- Path: ```exercises/error_handling/errors5.rs```
#### Hint: 

There are two different possible `Result` types produced within `main()`, which are
propagated using `?` operators. How do we declare a return type from `main()` that allows both?

Under the hood, the `?` operator calls `From::from` on the error value to convert it to a boxed
trait object, a `Box<dyn error::Error>`. This boxed trait object is polymorphic, and since all
errors implement the `error::Error` trait, we can capture lots of different errors in one "Box"
object.

Check out this section of the book:

[https://doc.rust-lang.org/...a-shortcut-for-propagating-errors-the--operator](https://doc.rust-lang.org/book/ch09-02-recoverable-errors-with-result.html#a-shortcut-for-propagating-errors-the--operator) ([Chinese version](https://rustwiki.org/zh-CN/book/ch09-02-recoverable-errors-with-result.html#%E4%BC%A0%E6%92%AD%E9%94%99%E8%AF%AF%E7%9A%84%E7%AE%80%E5%86%99-%E8%BF%90%E7%AE%97%E7%AC%A6))

Read more about boxing errors:

[https://doc.rust-lang.org/...boxing_errors.html](https://doc.rust-lang.org/stable/rust-by-example/error/multiple_error_types/boxing_errors.html) ([Chinese version](https://rustwiki.org/zh-CN/rust-by-example/error/multiple_error_types/boxing_errors.html))

Read more about using the `?` operator with boxed errors:

[https://doc.rust-lang.org/...reenter_question_mark.html](https://doc.rust-lang.org/stable/rust-by-example/error/multiple_error_types/reenter_question_mark.html) ([Chinese version](https://rustwiki.org/zh-CN/rust-by-example/error/multiple_error_types/reenter_question_mark.html))

---

```rust,editable
// errors5.rs
//
// This program uses an altered version of the code from errors4.
//
// This exercise uses some concepts that we won't get to until later in the
// course, like `Box` and the `From` trait. It's not important to understand
// them in detail right now, but you can read ahead if you like. For now, think
// of the `Box<dyn ???>` type as an "I want anything that does ???" type, which,
// given Rust's usual standards for runtime safety, should strike you as
// somewhat lenient!
//
// In short, this particular use case for boxes is for when you want to own a
// value and you care only that it is a type which implements a particular
// trait. To do so, The Box is declared as of type Box<dyn Trait> where Trait is
// the trait the compiler looks for on any value used in that context. For this
// exercise, that context is the potential errors which can be returned in a
// Result.
//
// What can we use to describe both errors? In other words, is there a trait
// which both errors implement?
//
// Execute `rustlings hint errors5` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

use std::error;
use std::fmt;
use std::num::ParseIntError;

// TODO: update the return type of `main()` to make this compile.
fn main() -> Result<(), Box<dyn ???>> {
    let pretend_user_input = "42";
    let x: i64 = pretend_user_input.parse()?;
    println!("output={:?}", PositiveNonzeroInteger::new(x)?);
    Ok(())
}

// Don't change anything below this line.

#[derive(PartialEq, Debug)]
struct PositiveNonzeroInteger(u64);

#[derive(PartialEq, Debug)]
enum CreationError {
    Negative,
    Zero,
}

impl PositiveNonzeroInteger {
    fn new(value: i64) -> Result<PositiveNonzeroInteger, CreationError> {
        match value {
            x if x < 0 => Err(CreationError::Negative),
            x if x == 0 => Err(CreationError::Zero),
            x => Ok(PositiveNonzeroInteger(x as u64)),
        }
    }
}

// This is required so that `CreationError` can implement `error::Error`.
impl fmt::Display for CreationError {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        let description = match *self {
            CreationError::Negative => "number is negative",
            CreationError::Zero => "number is zero",
        };
        f.write_str(description)
    }
}

impl error::Error for CreationError {}

```

---

### 本题内容

本题目要求通过使用 `Box<dyn error::Error>` 来处理 `main` 函数中可能出现的两种不同的 `Result` 类型。目的是学习如何在一个函数返回类型中同时处理多种错误类型。

### 相关知识点

1. **多态错误处理**：使用 `Box<dyn error::Error>` 可以在 Rust 中实现多态错误处理，即一个函数能够返回多种错误类型。
2. **`Box<dyn Trait>` 类型**：在 Rust 中，`Box<dyn Trait>` 表示一个装箱的值，这个值实现了 `Trait` 特征。`Box` 允许我们在堆上分配内存，而 `dyn Trait` 表示任何实现了 `Trait` 特征的类型。
3. **`?` 操作符和错误转换**：`?` 操作符可以用于 `Result` 类型的错误传播。当错误发生时，它会自动将错误转换为函数返回类型所期望的错误类型，这是通过 `From::from` 实现的。

### 解题方法

1. **更新 `main` 函数的返回类型**：根据题目提示，我们知道需要将 `main` 函数的返回类型更新为 `Result<(), Box<dyn error::Error>>`。
2. **理解 `Box<dyn error::Error>` 的作用**：这样做是为了允许 `main` 函数返回任何实现了 `error::Error` 特征的错误类型。由于 `ParseIntError` 和 `CreationError` 都实现了 `error::Error` 特征，它们都可以被装箱并作为错误返回。

### 代码示例

根据上述解题方法，这是修改后的代码：

```rust
use std::error;
use std::fmt;
use std::num::ParseIntError;

// 更新 main 函数的返回类型
fn main() -> Result<(), Box<dyn error::Error>> {
    let pretend_user_input = "42";
    let x: i64 = pretend_user_input.parse()?;
    println!("output={:?}", PositiveNonzeroInteger::new(x)?);
    Ok(())
}

// 下面的代码保持不变
#[derive(PartialEq, Debug)]
struct PositiveNonzeroInteger(u64);

#[derive(PartialEq, Debug)]
enum CreationError {
    Negative,
    Zero,
}

impl PositiveNonzeroInteger {
    fn new(value: i64) -> Result<PositiveNonzeroInteger, CreationError> {
        match value {
            x if x < 0 => Err(CreationError::Negative),
            x if x == 0 => Err(CreationError::Zero),
            x => Ok(PositiveNonzeroInteger(x as u64)),
        }
    }
}

impl fmt::Display for CreationError {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        let description = match *self {
            CreationError::Negative => "number is negative",
            CreationError::Zero => "number is zero",
        };
        f.write_str(description)
    }
}

impl error::Error for CreationError {}
```

在这个修改中：
- `main` 函数的返回类型被更新为 `Result<(), Box<dyn error::Error>>`，这允许它返回多种错误类型。
- 由于 `?` 操作符的使用，当 `parse` 方法或 `PositiveNonzeroInteger::new` 方法返回错误时，这些错误将自动被转换为 `Box<dyn error::Error>` 类型并被传播出 `main` 函数。

通过这个练习，你不仅学习了如何处理多种错误类型，还理解了 `Box<dyn Trait>` 的使用和 `?` 操作符在错误处理中的作用。

## 扩展知识点与解答：

### 扩展知识点

1. **`dyn Error` 和类型擦除**：`dyn Error` 表示任何实现了 `Error` trait 的类型，这是一种使用动态分发的多态。类型擦除允许我们在编译时忽略具体的类型信息，仅仅关心值满足某个接口或trait，这种方式在处理多种错误类型时特别有用。

2. **特征对象（Trait Objects）**：在 Rust 中，特征对象提供了一种在运行时处理不同类型的通用方式。通过将错误装箱成 `Box<dyn Error>`，我们可以在不知道具体错误类型的情况下，返回或处理多种错误。

3. **错误转换（Error Conversion）**：Rust 的 `From` trait 用于在不同类型之间进行转换，这在错误处理中特别有用。当使用 `?` 操作符时，Rust 会尝试使用 `From::from` 方法将原始错误转换成目标错误类型，这使得错误传播变得非常灵活和强大。

4. **错误处理的模式和实践**：在复杂的应用中，错误处理策略的选择和实现变得尤为重要。学习如何设计和实现灵活的错误处理模式，可以帮助开发者更好地控制程序的错误流和用户体验。

### 扩展解题方法

1. **自定义错误类型和层次**：在大型项目中，定义一个清晰的错误类型层次结构，可以帮助更好地组织和管理错误。通过实现自定义错误类型，并利用 `From` trait 进行错误类型之间的转换，可以使错误处理更加灵活和统一。

2. **错误处理策略**：根据应用的具体需求，开发合适的错误处理策略。例如，某些情况下忽略错误可能是可接受的，而在其他情况下，可能需要将错误记录到日志并通知用户。明确何时应该停止错误传播，何时应该恢复错误，并采取相应的动作。

3. **使用第三方库**：探索和使用第三方错误处理库，如 `anyhow` 和 `thiserror`。这些库提供了额外的功能和便利，例如简化的错误定义、自动的 `From` trait 实现、以及更丰富的上下文信息。

4. **深入理解 `Box<dyn Error>` 的使用场景**：虽然 `Box<dyn Error>` 在很多情况下都非常有用，但它也有一些限制，比如性能开销和缺乏类型信息。理解何时使用它，以及何时寻找其他解决方案，是高效错误处理的关键。

### 应用到当前练习

在当前的 `errors5` 练习中，通过扩展探索如何构建一个灵活且强大的错误处理系统，这对于构建大型、可维护的 Rust 应用来说是非常重要的。掌握多态错误处理、错误类型转换，以及如何设计合理的错误处理策略，将有助于提高代码的质量和用户体验。此外，考虑到 Rust 社区提供的丰富错误处理工具和库，积极探索和采用这些工具，可以进一步提升错误处理的效率和效果。

### 为什么使用 `Box<dyn Error>` 而不是泛型

这个问题触及了 Rust 错误处理中的两个重要概念：多态和所有权。理解为什么在某些情况下使用 `Box<dyn Error>` 而不是泛型，可以从这两点着手考虑。

#### 多态性

当函数可能返回多种不同类型的错误时，我们希望这些错误都能以某种方式被统一处理。这里的关键是多态性——我们需要一个能够代表任何错误的通用类型。Rust 中的 `dyn Error` trait 提供了这样的多态性：任何实现了 `Error` trait 的类型都可以作为错误返回。

#### 所有权与大小

Rust 需要在编译时知道每个类型的大小。泛型类型 `T` 允许在编译时确定每个具体类型的大小，这意味着当你使用泛型返回类型时，每种可能的返回类型都必须是大小已知且相同的。然而，错误类型可能有着不同的大小，且在编译时我们无法确定将要处理哪种错误。

使用 `Box<dyn Error>` 可以解决这个问题，因为它使用了堆分配。`Box<T>` 是一个指针类型，所有指针的大小在编译时都是已知的（例如，在 64 位系统上通常是 64 位）。这意味着无论 `T` 实际上有多大，`Box<T>` 的大小都是固定的。这样，我们就可以返回一个指向任意实现了 `Error` trait 的类型的指针，而无需在编译时知道具体的错误类型。

#### 泛型限制

如果我们尝试使用泛型来实现相同的功能，我们会遇到一些限制：

1. **代码膨胀**：对于使用泛型的每种具体类型，Rust 需要生成新的函数或类型实例。如果一个函数返回多种不同的错误类型，使用泛型可能会导致大量的代码重复生成。

2. **灵活性**：使用泛型时，调用者必须指定具体的类型，这减少了代码的灵活性。相反，`Box<dyn Error>` 允许函数返回任何错误类型，使得 API 更加灵活。

3. **复杂性**：尽管 Rust 的泛型系统非常强大，但在错误处理的上下文中使用泛型会增加 API 的复杂性。特别是对于库的作者，提供一个简单的、基于 `Box<dyn Error>` 的 API 可能更容易让人理解和使用。

总的来说，使用 `Box<dyn Error>` 是一种在保持代码灵活性和简洁性的同时，处理多种不同错误类型的有效方法。尽管它引入了一些性能开销（如堆分配和动态分发），但在很多场景中，这种开销是可以接受的。
