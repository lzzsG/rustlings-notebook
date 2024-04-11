# Exercise 56

- Name: ```errors6```
- Path: ```exercises/error_handling/errors6.rs```
#### Hint: 

This exercise uses a completed version of `PositiveNonzeroInteger` from errors4.

Below the line that TODO asks you to change, there is an example of using the `map_err()` method on a `Result` to transform one type of error into another. Try using something similar on the `Result` from `parse()`. You might use the `?` operator to return early from the function, or you might use a `match` expression, or maybe there's another way!

You can create another function inside `impl ParsePosNonzeroError` to use
with `map_err()`.

Read more about `map_err()` in the `std::result` documentation:

[https://doc.rust-lang.org/std/result/enum.Result.html#method.map_err](https://doc.rust-lang.org/std/result/enum.Result.html#method.map_err) ([Chinese version]())


---



```rust,editable
// errors6.rs
//
// Using catch-all error types like `Box<dyn error::Error>` isn't recommended
// for library code, where callers might want to make decisions based on the
// error content, instead of printing it out or propagating it further. Here, we
// define a custom error type to make it possible for callers to decide what to
// do next when our function returns an error.
//
// Execute `rustlings hint errors6` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

use std::num::ParseIntError;

// This is a custom error type that we will be using in `parse_pos_nonzero()`.
#[derive(PartialEq, Debug)]
enum ParsePosNonzeroError {
    Creation(CreationError),
    ParseInt(ParseIntError),
}

impl ParsePosNonzeroError {
    fn from_creation(err: CreationError) -> ParsePosNonzeroError {
        ParsePosNonzeroError::Creation(err)
    }
    // TODO: add another error conversion function here.
    // fn from_parseint...
}

fn parse_pos_nonzero(s: &str) -> Result<PositiveNonzeroInteger, ParsePosNonzeroError> {
    // TODO: change this to return an appropriate error instead of panicking
    // when `parse()` returns an error.
    let x: i64 = s.parse().unwrap();
    PositiveNonzeroInteger::new(x).map_err(ParsePosNonzeroError::from_creation)
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

#[cfg(test)]
mod test {
    use super::*;

    #[test]
    fn test_parse_error() {
        // We can't construct a ParseIntError, so we have to pattern match.
        assert!(matches!(
            parse_pos_nonzero("not a number"),
            Err(ParsePosNonzeroError::ParseInt(_))
        ));
    }

    #[test]
    fn test_negative() {
        assert_eq!(
            parse_pos_nonzero("-555"),
            Err(ParsePosNonzeroError::Creation(CreationError::Negative))
        );
    }

    #[test]
    fn test_zero() {
        assert_eq!(
            parse_pos_nonzero("0"),
            Err(ParsePosNonzeroError::Creation(CreationError::Zero))
        );
    }

    #[test]
    fn test_positive() {
        let x = PositiveNonzeroInteger::new(42);
        assert!(x.is_ok());
        assert_eq!(parse_pos_nonzero("42"), Ok(x.unwrap()));
    }
}

```

---

### 本题内容

这个练习的目的是让学生学习如何定义和使用自定义错误类型来处理多种错误情况。通过这个过程，学生将理解如何在 Rust 中优雅地处理不同来源的错误，以及如何将它们转换为统一的错误类型，使得错误更容易管理和使用。

### 相关知识点

1. **自定义错误类型**：定义一个枚举来表示函数可能返回的不同错误类型。
2. **错误转换**：使用 `map_err` 方法将一个错误类型转换为另一个错误类型。
3. **`Result` 类型**：`Result` 是 Rust 中用于错误处理的枚举，包含 `Ok(T)` 和 `Err(E)` 两个变体。
4. **`match` 表达式**：一种强大的控制流工具，可用于根据枚举变体执行不同的代码分支。

### 解题方法

1. **实现错误转换函数**：在 `ParsePosNonzeroError` 枚举中实现一个从 `ParseIntError` 到 `ParsePosNonzeroError` 的转换函数。
2. **修改 `parse_pos_nonzero` 函数**：使用 `map_err` 来转换 `parse` 方法产生的 `ParseIntError` 至自定义错误类型 `ParsePosNonzeroError`。

### 代码示例

首先，实现从 `ParseIntError` 到 `ParsePosNonzeroError` 的转换函数：

```rust
impl ParsePosNonzeroError {
    // 已有的从 CreationError 到 ParsePosNonzeroError 的转换
    fn from_creation(err: CreationError) -> ParsePosNonzeroError {
        ParsePosNonzeroError::Creation(err)
    }

    // 新的从 ParseIntError 到 ParsePosNonzeroError 的转换
    fn from_parseint(err: ParseIntError) -> ParsePosNonzeroError {
        ParsePosNonzeroError::ParseInt(err)
    }
}
```

然后，修改 `parse_pos_nonzero` 函数来使用这个新的转换函数，并避免使用 `unwrap`：

```rust
fn parse_pos_nonzero(s: &str) -> Result<PositiveNonzeroInteger, ParsePosNonzeroError> {
    // 使用 map_err 转换 parse 方法产生的 ParseIntError
    let x: i64 = s.parse().map_err(ParsePosNonzeroError::from_parseint)?;
    PositiveNonzeroInteger::new(x).map_err(ParsePosNonzeroError::from_creation)
}
```

这样修改后，`parse_pos_nonzero` 函数现在能够优雅地处理字符串解析错误和创建正非零整数的错误，将它们统一转换为 `ParsePosNonzeroError` 类型，并通过 `Result` 返回。这种错误处理方式既清晰又灵活，允许调用者根据错误类型做出相应的决策。

## 扩展知识点与解答：

### 扩展知识点

1. **错误处理的最佳实践**：理解何时应该定义自定义错误类型，以及如何利用 Rust 的类型系统和特征（traits）来创建可靠且易于维护的错误处理逻辑。

2. **`From` 特征用于错误转换**：Rust 标准库中的 `From` 特征可以用于在错误类型之间进行转换。实现 `From<T>` 为你的错误类型意味着你可以使用 `?` 操作符直接将类型 `T` 的错误转换为你的错误类型。

3. **错误链**：在复杂的应用中，错误可能会经过多个层次的传递和转换。Rust 1.30 以上版本中的 `source` 方法允许你访问原始错误，从而提供了一种创建错误链的方法，这对于调试和错误报告非常有用。

4. **使用第三方库处理复杂错误**：像 `anyhow` 和 `thiserror` 这样的第三方库提供了更高级的错误处理能力，包括自动从底层错误到自定义错误的转换，以及更简单的方式来创建错误链。

### 扩展解题方法

1. **实现 `From` 特征以简化错误转换**：对于 `ParsePosNonzeroError`，你可以为每个可能的错误源（如 `ParseIntError` 和 `CreationError`）实现 `From` 特征。这样，你可以在函数中更方便地使用 `?` 操作符，而不需要显式调用 `map_err`。

   ```rust
   impl From<ParseIntError> for ParsePosNonzeroError {
       fn from(err: ParseIntError) -> Self {
           ParsePosNonzeroError::ParseInt(err)
       }
   }

   impl From<CreationError> for ParsePosNonzeroError {
       fn from(err: CreationError) -> Self {
           ParsePosNonzeroError::Creation(err)
       }
   }
   ```

2. **深入理解和使用错误链**：为你的自定义错误类型实现 `source` 方法，返回底层错误的引用（如果有的话）。这使得终端用户和应用程序能够更好地理解错误发生的上下文。

3. **探索和采纳第三方错误处理库**：考虑在你的项目中使用如 `anyhow` 或 `thiserror` 这样的库，以进一步简化错误处理逻辑。`anyhow` 提供了一个通用的错误类型，适合用于应用程序中，而 `thiserror` 更适合用于库的错误处理，因为它支持自定义错误类型。

通过上述扩展知识点和解题方法，你不仅可以解决当前的练习问题，还能在日常的 Rust 编程实践中更加高效地处理错误。这些高级技巧和工具将有助于提升你的 Rust 错误处理能力，使得你能够构建更健壮、易于维护的 Rust 应用和库。

###  `anyhow` 和 `thiserror` 库示例

#### 示例1：

使用 `anyhow` 库能够极大简化 Rust 中的错误处理，特别是在应用程序开发中。`anyhow` 提供了一个方便的 `Error` 类型用于表示几乎所有种类的错误，并支持自动错误转换和错误链。这里我们通过一个简单的例子来展示 `anyhow` 的基本用法。

首先，你需要在 `Cargo.toml` 中添加 `anyhow` 依赖：

```toml
[dependencies]
anyhow = "1.0"
```

假设我们有一个函数 `parse_config_file`，它尝试解析一个配置文件，该文件可能不存在，也可能内容格式不正确。我们希望在发生任何错误时返回一个包含错误信息的 `Result`。

不使用 `anyhow`，你可能需要定义多个错误类型或者使用 `Box<dyn Error>`。使用 `anyhow`，你可以直接返回 `anyhow::Result<T>`，它是 `Result<T, anyhow::Error>` 的别名：

```rust
use anyhow::{Context, Result};
use std::fs;

fn parse_config_file(path: &str) -> Result<String> {
    // 读取文件内容。如果文件不存在，`fs::read_to_string` 返回的错误将被转换为 `anyhow::Error`。
    let content = fs::read_to_string(path).context("Failed to read config file")?;

    // 假设我们对文件内容进行一些解析...
    if content.is_empty() {
        // 直接使用 `anyhow::bail!` 宏来快速返回错误
        anyhow::bail!("Config file is empty");
    }

    // 如果一切正常，返回文件内容
    Ok(content)
}

fn main() {
    match parse_config_file("config.toml") {
        Ok(content) => println!("Config content: {}", content),
        Err(err) => println!("Error: {:?}", err),
    }
}
```

在这个例子中，我们使用了 `anyhow::Result` 作为函数的返回类型，这让错误处理变得非常简单。`context` 方法允许我们为潜在的错误添加上下文信息，这对于调试和错误报告非常有用。如果有错误发生，`anyhow` 能够自动从几乎所有的错误类型转换成 `anyhow::Error`，保留原始错误信息和发生错误的上下文。`anyhow::bail!` 宏提供了一种快速从函数中返回错误的方式。

#### 示例2：

为了展示 `anyhow` 在处理更复杂的自定义错误情况下的能力，我们可以定义一个自定义错误类型，并使用 `thiserror` 库来简化错误类型的实现。然后，我们将使用 `anyhow` 来包装和传播这些自定义错误。

首先，添加 `anyhow` 和 `thiserror` 到你的 `Cargo.toml`：

```toml
[dependencies]
anyhow = "1.0"
thiserror = "1.0"
```

假设我们正在编写一个简单的应用，该应用需要从网络上加载数据，并解析这些数据。我们可以定义一些特定的错误类型来表示可能发生的错误情况：

```rust
use thiserror::Error;
use anyhow::Result;

// 定义我们自己的错误类型
#[derive(Error, Debug)]
enum DataError {
    #[error("network error: {0}")]
    Network(String),

    #[error("parse error: {0}")]
    Parse(String),

    #[error("data not found")]
    NotFound,
}

// 假设这是一个获取数据的函数
fn fetch_data(url: &str) -> Result<String, DataError> {
    if url == "" {
        return Err(DataError::Network("URL cannot be empty".into()));
    }

    // 这里省略了实际的网络请求代码，直接返回一个错误
    Err(DataError::NotFound)
}

// 处理数据的函数，它会尝试获取数据然后解析
fn process_data(url: &str) -> Result<()> {
    let data = fetch_data(url).map_err(anyhow::Error::new)?;

    // 假设这里有解析数据的逻辑，但我们直接返回一个解析错误
    Err(DataError::Parse("Invalid data format".into()).into())
}

fn main() {
    match process_data("http://example.com/data") {
        Ok(_) => println!("Data processed successfully"),
        Err(err) => println!("Error: {:?}", err),
    }
}
```

在这个例子中：

1. 我们定义了一个 `DataError` 枚举，用于表示不同的错误情况。我们利用 `thiserror` 库来自动生成 `Error` trait 的实现。
2. 在 `fetch_data` 函数中，我们直接返回定义的错误类型。在 `process_data` 函数中，我们尝试获取并处理数据。如果发生错误，我们使用 `anyhow::Error::new` 将自定义错误转换为 `anyhow::Error`，这样就可以利用 `anyhow` 提供的错误链和上下文管理特性。
3. 在 `main` 函数中，我们处理 `process_data` 函数的结果。使用 `anyhow`，我们可以轻松地打印出完整的错误信息和上下文，甚至如果有错误链，`anyhow` 也能够展示出完整的链路。

这个例子展示了如何将 `anyhow` 和 `thiserror` 结合使用来处理复杂的错误场景，使得错误处理既灵活又强大。通过定义自定义错误并利用 `anyhow` 来传播和管理这些错误，我们可以构建出既清晰又健壮的错误处理逻辑。

#### 示例3：

当你需要处理复杂的错误场景，特别是在库或大型应用中，`thiserror` 库提供了一个简洁且强大的方式来定义和使用自定义错误。`thiserror` 的核心优势在于它能自动实现 `std::error::Error` trait，而且支持嵌套错误和自动从源错误类型转换。

以下示例演示了 `thiserror` 的一些高级用法，包括错误嵌套、源错误自动转换和条件编译。

首先，在 `Cargo.toml` 中添加 `thiserror` 依赖：

```toml
[dependencies]
thiserror = "1.0"
```

然后，定义一系列复杂的错误类型：

```rust
use thiserror::Error;
use std::fmt;

// 假设的外部错误，用于模拟从其他库引入的错误类型
#[derive(Debug, Clone)]
struct ExternalError {
    message: String,
}

impl fmt::Display for ExternalError {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        write!(f, "External error: {}", self.message)
    }
}

// 我们自定义的错误类型
#[derive(Error, Debug)]
pub enum MyError {
    #[error("data not available")]
    DataNotAvailable,
    #[error("data format error")]
    DataFormatError,
    #[error("network error: {0}")]
    NetworkError(String),
    // 嵌套错误，自动从 ExternalError 转换
    #[error(transparent)]
    External(#[from] ExternalError),
    // 使用条件编译仅在测试环境下包含的错误类型
    #[cfg(test)]
    #[error("test error")]
    TestError,
}

// 模拟一个可能失败的函数，返回 Result 类型
fn might_fail(flag: i32) -> Result<(), MyError> {
    match flag {
        1 => Err(MyError::DataNotAvailable),
        2 => Err(MyError::DataFormatError),
        3 => Err(MyError::NetworkError("timeout".into())),
        4 => Err(ExternalError { message: "external service error".into() }.into()),
        #[cfg(test)]
        5 => Err(MyError::TestError),
        _ => Ok(()),
    }
}

fn main() {
    match might_fail(4) {
        Ok(_) => println!("Operation succeeded"),
        Err(e) => println!("Error occurred: {}", e),
    }
}
```

在这个例子中：

- 我们定义了一个名为 `MyError` 的自定义错误类型，它包含了多个错误的变体，这些变体代表不同的错误情况。
- 使用 `#[error(transparent)]` 属性标记的 `External` 变体展示了 `thiserror` 如何支持错误嵌套和自动从源错误转换。这意味着你可以从 `ExternalError` 类型的错误自动转换为 `MyError` 类型的错误。
- 我们还展示了如何使用 `#[cfg(test)]` 属性来条件编译错误类型，使得某些错误仅在测试环境下可用。
- 在 `might_fail` 函数中，我们模拟了不同的失败情况，演示了如何返回我们定义的错误类型。
- 在 `main` 函数中，我们处理 `might_fail` 函数的返回值，展示了如何打印错误信息。

通过这个示例，你可以看到 `thiserror` 提供的自定义错误定义方法非常强大且灵活，特别是在需要处理多层次错误或跨库错误时。利用 `thiserror`，你可以构建清晰、可维护的错误处理逻辑，同时保持代码的简洁。
