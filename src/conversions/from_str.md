# Exercise 93

- Name: ```from_str```
- Path: ```exercises/conversions/from_str.rs```
#### Hint: 

The implementation of FromStr should return an Ok with a Person object, or an Err with an error if the string is not valid.

This is almost like the `from_into` exercise, but returning errors instead of falling back to a default value.

Look at the test cases to see which error variants to return.

Another hint: You can use the `map_err` method of `Result` with a function or a closure to wrap the error from `parse::<usize>`.

Yet another hint: If you would like to propagate errors by using the `?` operator in your solution, you might want to look at
[https://doc.rust-lang.org/stable/rust-by-example/error /multiple_error_types/reenter_question_mark.html](https://doc.rust-lang.org/stable/rust-by-example/error/multiple_error_types/reenter_question_mark.html) ([Chinese version](https://rustwiki.org/zh-CN/rust-by-example/error/multiple_error_types/reenter_question_mark.html))



---



```rust,editable
// from_str.rs
//
// This is similar to from_into.rs, but this time we'll implement `FromStr` and
// return errors instead of falling back to a default value. Additionally, upon
// implementing FromStr, you can use the `parse` method on strings to generate
// an object of the implementor type. You can read more about it at
// https://doc.rust-lang.org/std/str/trait.FromStr.html
//
// Execute `rustlings hint from_str` or use the `hint` watch subcommand for a
// hint.

use std::num::ParseIntError;
use std::str::FromStr;

#[derive(Debug, PartialEq)]
struct Person {
    name: String,
    age: usize,
}

// We will use this error type for the `FromStr` implementation.
#[derive(Debug, PartialEq)]
enum ParsePersonError {
    // Empty input string
    Empty,
    // Incorrect number of fields
    BadLen,
    // Empty name field
    NoName,
    // Wrapped error from parse::<usize>()
    ParseInt(ParseIntError),
}

// I AM NOT DONE

// Steps:
// 1. If the length of the provided string is 0, an error should be returned
// 2. Split the given string on the commas present in it
// 3. Only 2 elements should be returned from the split, otherwise return an
//    error
// 4. Extract the first element from the split operation and use it as the name
// 5. Extract the other element from the split operation and parse it into a
//    `usize` as the age with something like `"4".parse::<usize>()`
// 6. If while extracting the name and the age something goes wrong, an error
//    should be returned
// If everything goes well, then return a Result of a Person object
//
// As an aside: `Box<dyn Error>` implements `From<&'_ str>`. This means that if
// you want to return a string error message, you can do so via just using
// return `Err("my error message".into())`.

impl FromStr for Person {
    type Err = ParsePersonError;
    fn from_str(s: &str) -> Result<Person, Self::Err> {
    }
}

fn main() {
    let p = "Mark,20".parse::<Person>().unwrap();
    println!("{:?}", p);
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn empty_input() {
        assert_eq!("".parse::<Person>(), Err(ParsePersonError::Empty));
    }
    #[test]
    fn good_input() {
        let p = "John,32".parse::<Person>();
        assert!(p.is_ok());
        let p = p.unwrap();
        assert_eq!(p.name, "John");
        assert_eq!(p.age, 32);
    }
    #[test]
    fn missing_age() {
        assert!(matches!(
            "John,".parse::<Person>(),
            Err(ParsePersonError::ParseInt(_))
        ));
    }

    #[test]
    fn invalid_age() {
        assert!(matches!(
            "John,twenty".parse::<Person>(),
            Err(ParsePersonError::ParseInt(_))
        ));
    }

    #[test]
    fn missing_comma_and_age() {
        assert_eq!("John".parse::<Person>(), Err(ParsePersonError::BadLen));
    }

    #[test]
    fn missing_name() {
        assert_eq!(",1".parse::<Person>(), Err(ParsePersonError::NoName));
    }

    #[test]
    fn missing_name_and_age() {
        assert!(matches!(
            ",".parse::<Person>(),
            Err(ParsePersonError::NoName | ParsePersonError::ParseInt(_))
        ));
    }

    #[test]
    fn missing_name_and_invalid_age() {
        assert!(matches!(
            ",one".parse::<Person>(),
            Err(ParsePersonError::NoName | ParsePersonError::ParseInt(_))
        ));
    }

    #[test]
    fn trailing_comma() {
        assert_eq!("John,32,".parse::<Person>(), Err(ParsePersonError::BadLen));
    }

    #[test]
    fn trailing_comma_and_some_string() {
        assert_eq!(
            "John,32,man".parse::<Person>(),
            Err(ParsePersonError::BadLen)
        );
    }
}

```

---

### 本题内容
本练习旨在实现 `FromStr` 特质来从字符串解析出一个 `Person` 结构体。这个过程涉及错误处理，当字符串格式不正确时返回一个错误而不是使用默认值。这对于提高程序的健壮性和正确性非常重要。

### 相关知识点
- **`FromStr` 特质**：这是 Rust 中用于定义如何将字符串解析成特定类型的方法。它通常与 `parse` 方法结合使用，当类型实现了 `FromStr` 特质后，就可以直接使用 `parse` 方法将字符串转换为该类型。
  
- **错误处理**：在解析过程中，合理处理各种可能出现的错误情况是至关重要的。这包括但不限于输入为空、格式不正确、类型转换失败等。

- **类型转换与 `parse` 方法**：使用 `parse::<usize>()` 来将字符串转换为整数，并通过错误处理确保转换的安全性。

### 解题方法
1. **检查输入**：首先检查输入字符串是否为空，如果为空，则返回 `ParsePersonError::Empty` 错误。

2. **分割字符串**：使用逗号 `,` 将输入字符串分割成名字和年龄两部分。如果分割后的数组长度不为 2，返回 `ParsePersonError::BadLen`。

3. **解析名字**：从分割得到的数组中取出第一部分作为名字，如果名字为空，则返回 `ParsePersonError::NoName`。

4. **解析年龄**：从数组中取出第二部分，并尝试将其解析为 `usize`。如果解析失败，返回 `ParsePersonError::ParseInt` 错误，该错误内部包含解析失败的详细信息。

5. **创建并返回 `Person` 实例**：如果所有步骤都成功，使用解析出的名字和年龄创建一个 `Person` 实例并返回。

### 代码示例
完整的 `FromStr` 实现可能如下所示：

```rust
impl FromStr for Person {
    type Err = ParsePersonError;

    fn from_str(s: &str) -> Result<Self, Self::Err> {
        if s.is_empty() {
            return Err(ParsePersonError::Empty);
        }

        let parts: Vec<&str> = s.split(',').collect();
        if parts.len() != 2 {
            return Err(ParsePersonError::BadLen);
        }

        let name = parts[0].trim();
        if name.is_empty() {
            return Err(ParsePersonError::NoName);
        }

        let age = parts[1].trim().parse::<usize>()
            .map_err(ParsePersonError::ParseInt)?;

        Ok(Person {
            name: name.to_string(),
            age,
        })
    }
}
```

在 `main` 函数中，你可以使用这个实现来尝试解析一些输入：

```rust
fn main() {
    match "Alice,30".parse::<Person>() {
        Ok(person) => println!("Parsed: {:?}", person),
        Err(e) => println!("Error: {:?}", e),
    }
}
```

通过这样的实践，你可以深入理解如何在 Rust 中处理类型转换和错误管理，这对于编写健壮且可维护的 Rust 应用程序至关重要。


---

## 扩展知识点与解答：

### 扩展知识点

1. **错误传播与错误包装**：
   - 在 Rust 中，错误处理是通过 `Result` 类型进行的。在实现 `FromStr` 时，常见的做法是将低级别的错误（例如 `parse::<usize>()` 返回的错误）包装到自定义错误中。这使得错误信息更加丰富，也便于上层代码处理错误。
   - 使用 `map_err` 方法可以将错误从一种类型转换为另一种类型，这在错误处理中非常有用，特别是当你需要将库函数产生的错误转换为自定义错误类型时。

2. **泛型与类型推断**：
   - `FromStr` 特质是泛型的，它允许在同一程序中为多种类型实现字符串解析功能。这种泛型的使用增加了代码的复用性并提高了类型安全。

3. **模式匹配**：
   - 在 Rust 中，模式匹配是处理枚举（如 `Result` 或自定义错误类型）时的强大工具。通过模式匹配，你可以根据函数返回的不同结果执行不同的逻辑。

### 扩展解题方法

1. **详细的输入验证**：
   - 在实际应用中，除了基本的长度和内容检查外，可能还需要进行更复杂的验证，例如检查字符串是否包含非法字符、是否符合特定格式等。通过在解析函数中增加这些验证步骤，可以提高应用的健壮性。

2. **利用正则表达式进行解析**：
   - 对于更复杂的字符串格式，使用正则表达式可以提供一种更灵活、更强大的解析方法。例如，可以使用正则表达式来一次性验证字符串格式并提取所需的字段。

3. **错误处理策略的优化**：
   - 在设计错误类型时，可以考虑创建更详细的错误分类，如区分输入数据格式错误、类型转换错误等。这不仅有助于调试，也使得应用可以根据不同的错误类型给出更有针对性的用户反馈。

4. **单元测试的强化**：
   - 在实现 `FromStr` 特质时，编写全面的单元测试是非常重要的。测试不仅应覆盖各种正常情况，还应包括边界情况和异常情况，确保在各种意外输入下程序都能正确处理。

## 代码示例： 读取用户配置信息

下面是一个实际中可能使用的更复杂的 `FromStr` 实现的示例。这个示例包括自定义错误处理、使用正则表达式进行格式验证和提取，以及详尽的错误分类，用于解析一个具有多个字段的复杂配置字符串到一个结构体中。

### 场景描述

设想我们需要从配置文件中读取用户的配置信息，格式为 `"username:age:email"`，我们需要确保每部分都符合特定的格式要求：

- **username** - 非空，只包含字母和数字。
- **age** - 必须是数字。
- **email** - 必须包含一个`@`符号。

### 实现

首先，定义 `UserConfig` 结构体和相应的错误类型：

```rust
use std::str::FromStr;
use std::num::ParseIntError;
use regex::Regex;
use std::fmt;

// 定义用户配置信息的结构体
#[derive(Debug, PartialEq)]
struct UserConfig {
    username: String,
    age: usize,
    email: String,
}

// 自定义错误类型，用于处理从字符串解析用户配置时可能发生的错误
#[derive(Debug, PartialEq)]
enum ParseUserConfigError {
    EmptyInput,
    BadFormat,
    InvalidUsername,
    InvalidAge(ParseIntError),
    InvalidEmail,
}

// 为自定义错误类型实现 Display 特质，以便于错误信息的输出
impl fmt::Display for ParseUserConfigError {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "{:?}", self)
    }
}

// 实现 FromStr 特质用于从字符串解析到 UserConfig 类型
impl FromStr for UserConfig {
    type Err = ParseUserConfigError;

    fn from_str(s: &str) -> Result<Self, Self::Err> {
        if s.is_empty() {
            return Err(ParseUserConfigError::EmptyInput);
        }

        // 使用正则表达式匹配合适的输入格式
        let re = Regex::new(r"^([a-zA-Z0-9]+):(\d+):([a-zA-Z0-9]+@[a-zA-Z0-9]+\.[a-zA-Z]+)$").unwrap();
        if let Some(caps) = re.captures(s) {
            let username = caps.get(1).unwrap().as_str().to_string();
            let age = caps.get(2).unwrap().as_str().parse::<usize>()
                .map_err(ParseUserConfigError::InvalidAge)?;
            let email = caps.get(3).unwrap().as_str().to_string();

            Ok(UserConfig { username, age, email })
        } else {
            Err(ParseUserConfigError::BadFormat)
        }
    }
}

fn main() {
    // 尝试解析一个字符串到 UserConfig 类型
    let config = "alice:30:alice@example.com".parse::<UserConfig>();
    println!("{:?}", config);
}
```

### 单元测试

为了确保 `FromStr` 的实现正确处理各种情况，应该编写一系列的单元测试：

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_valid_input() {
        let config = "alice:30:alice@example.com".parse::<UserConfig>().unwrap();
        assert_eq!(config, UserConfig {
            username: "alice".to_string(),
            age: 30,
            email: "alice@example.com".to_string()
        });
    }

    #[test]
    fn test_invalid_age() {
        assert!("alice:thirty:alice@example.com".parse::<UserConfig>().is_err());
    }

    #[test]
    fn test_invalid_email() {
        assert!("alice:30:aliceexample.com".parse::<UserConfig>().is_err());
    }
}
```

这个例子展示了如何使用 Rust 中的 `FromStr` 特质来执行复杂的字符串解析任务，同时通过丰富的错误处理提供了明确的反馈，增强了代码的健壮性和易用性。
