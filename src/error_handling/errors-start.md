# error_handling

---

在接下来一系列的错误处理练习中，我们将深入探索 Rust 中的错误处理机制，学习如何使用 Rust 提供的强大工具来优雅地处理程序中可能出现的各种错误情况。Rust 的错误处理模型主要基于两个枚举：`Option` 和 `Result`。通过这些练习，你将了解如何有效地使用这些模型来编写更安全、更健壮的代码。

- **理解 `Option` 和 `Result`**：`Option<T>` 用于可能缺失的值，而 `Result<T, E>` 用于可能失败的操作，其中 `T` 表示成功时的返回类型，`E` 表示错误类型。
- **错误传播**：学习如何使用 `?` 操作符来简化错误处理，它允许你在遇到错误时早期返回错误，而不是使用嵌套的 `match` 语句。
- **自定义错误类型**：探索如何定义自定义错误类型以提高代码的可读性和可维护性。
- **错误转换**：了解如何使用 `From` 和 `Into` 特征以及 `map_err` 方法来转换错误类型，以适应不同的错误处理场景。

### 练习概览

- **errors1**：你将从一个使用 `Option` 的函数开始，修改它以使用 `Result` 来提供更多的错误信息。
- **errors2**：在这个练习中，你将处理字符串解析为数字时可能出现的错误，并学习如何使用 `?` 操作符来简化错误传播。
- **errors3**：探索如何在 `main` 函数中处理错误，实践将 `Result` 用作 `main` 函数的返回类型。
- **errors4**：你将修正一个总是返回 `Ok` 的函数，使其能够在检查失败时返回错误。
- **errors5**：这个练习介绍了如何处理多种错误类型，并将它们统一为一个通用的错误类型，使用 `Box<dyn Error>`。
- **errors6**：最后，你将深入自定义错误类型的使用，了解如何创建和转换复杂的错误类型以提供更丰富的错误信息。

通过这些练习，你不仅会学习到 Rust 中错误处理的基础知识，还会掌握一些高级技巧，如自定义错误类型的创建和使用，以及如何处理和转换多种错误类型。这些技能对于编写高质量的 Rust 代码至关重要。让我们开始吧！

---

```rust
// 你不必现在理解以下代码，不过你可以尝试运行它。

use std::num::ParseIntError;

#[derive(Debug)]
enum AdventureError {
    WrongPath(String),
    ParseError(ParseIntError),
}

impl From<ParseIntError> for AdventureError {
    fn from(err: ParseIntError) -> Self {
        AdventureError::ParseError(err)
    }
}

fn check_scroll(scroll: &str) -> Result<(), AdventureError> {
    if scroll == "ancient scroll" {
        Ok(())
    } else {
        Err(AdventureError::WrongPath(format!(
            "{} is not the right scroll!",
            scroll
        )))
    }
}

fn parse_scroll_number(number_str: &str) -> Result<i32, AdventureError> {
    number_str.parse().map_err(AdventureError::from)
}

fn main() {
    let scroll = "Tattered Scroll";
    let number_str = "Broken numbers";

    match check_scroll(scroll) {
        Ok(_) => println!("Scroll check passed."),
        Err(e) => {
            println!("An error occurred: {:?}", e); 
        }
    };

    match parse_scroll_number(number_str) {
        Ok(number) => println!("The scroll guides you to path number {}.", number),
        Err(e) => {
            println!("An error occurred: {:?}", e); 
        }
    };

    let scroll = "ancient scroll";
    let number_str = "42";
    if let Err(e) = check_scroll(scroll) {
        println!("An error occurred: {:?}", e);
    }
    match check_scroll(scroll) {
        Ok(_) => println!("Replaced the scroll. Scroll check passed."),
        Err(e) => {
            println!("An error occurred: {:?}", e);
        }
    };

    match parse_scroll_number(number_str) {
        Ok(number) => println!("The scroll guides you to path number {}.", number),
        Err(e) => {
            println!("An error occurred: {:?}", e); 
        }
    };
    match parse_scroll_number(number_str) {
        Ok(number) => {
            if number == 42 {
                println!("You have found the mystical door, as foretold by the ancient scroll.🚪");
            } else {
                println!("This path leads nowhere. You need to check the scroll again.");
            }
        }
        Err(e) => println!("An error occurred: {:?}", e),
    }

    println!("\nYour journey continues into the🌲🌲🌲dark forest🌲🌲🌲\nA gentle breeze carries the scent of blooming night flowers, hinting at hidden \nwonders in the dark forest.");
}

```
