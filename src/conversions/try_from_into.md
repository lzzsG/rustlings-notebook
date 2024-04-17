# Exercise 94

- Name: ```try_from_into```
- Path: ```exercises/conversions/try_from_into.rs```
#### Hint: 

Follow the steps provided right before the `TryFrom` implementation. You can also use the example at[ https://doc.rust-lang.org/std/convert/trait.TryFrom.html]( https://doc.rust-lang.org/std/convert/trait.TryFrom.html) ([Chinese version]())

Is there an implementation of `TryFrom` in the standard library that can both do the required integer conversion and check the range of the input?

Another hint: Look at the test cases to see which error variants to return.

Yet another hint: You can use the `map_err` or `or` methods of `Result` to convert errors.

Yet another hint: If you would like to propagate errors by using the `?` operator in your solution, you might want to look at [https://doc.rust-lang.org/stable/rust-by-example/error/multiple_error_types/reenter_question_mark.html](https://doc.rust-lang.org/stable/rust-by-example/error/multiple_error_types/reenter_question_mark.html) ([Chinese version]())

Challenge: Can you make the `TryFrom` implementations generic over many integer types?


---



```rust,editable
// try_from_into.rs
//
// TryFrom is a simple and safe type conversion that may fail in a controlled
// way under some circumstances. Basically, this is the same as From. The main
// difference is that this should return a Result type instead of the target
// type itself. You can read more about it at
// https://doc.rust-lang.org/std/convert/trait.TryFrom.html
//
// Execute `rustlings hint try_from_into` or use the `hint` watch subcommand for
// a hint.

use std::convert::{TryFrom, TryInto};

#[derive(Debug, PartialEq)]
struct Color {
    red: u8,
    green: u8,
    blue: u8,
}

// We will use this error type for these `TryFrom` conversions.
#[derive(Debug, PartialEq)]
enum IntoColorError {
    // Incorrect length of slice
    BadLen,
    // Integer conversion error
    IntConversion,
}

// I AM NOT DONE

// Your task is to complete this implementation and return an Ok result of inner
// type Color. You need to create an implementation for a tuple of three
// integers, an array of three integers, and a slice of integers.
//
// Note that the implementation for tuple and array will be checked at compile
// time, but the slice implementation needs to check the slice length! Also note
// that correct RGB color values must be integers in the 0..=255 range.

// Tuple implementation
impl TryFrom<(i16, i16, i16)> for Color {
    type Error = IntoColorError;
    fn try_from(tuple: (i16, i16, i16)) -> Result<Self, Self::Error> {
    }
}

// Array implementation
impl TryFrom<[i16; 3]> for Color {
    type Error = IntoColorError;
    fn try_from(arr: [i16; 3]) -> Result<Self, Self::Error> {
    }
}

// Slice implementation
impl TryFrom<&[i16]> for Color {
    type Error = IntoColorError;
    fn try_from(slice: &[i16]) -> Result<Self, Self::Error> {
    }
}

fn main() {
    // Use the `try_from` function
    let c1 = Color::try_from((183, 65, 14));
    println!("{:?}", c1);

    // Since TryFrom is implemented for Color, we should be able to use TryInto
    let c2: Result<Color, _> = [183, 65, 14].try_into();
    println!("{:?}", c2);

    let v = vec![183, 65, 14];
    // With slice we should use `try_from` function
    let c3 = Color::try_from(&v[..]);
    println!("{:?}", c3);
    // or take slice within round brackets and use TryInto
    let c4: Result<Color, _> = (&v[..]).try_into();
    println!("{:?}", c4);
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_tuple_out_of_range_positive() {
        assert_eq!(
            Color::try_from((256, 1000, 10000)),
            Err(IntoColorError::IntConversion)
        );
    }
    #[test]
    fn test_tuple_out_of_range_negative() {
        assert_eq!(
            Color::try_from((-1, -10, -256)),
            Err(IntoColorError::IntConversion)
        );
    }
    #[test]
    fn test_tuple_sum() {
        assert_eq!(
            Color::try_from((-1, 255, 255)),
            Err(IntoColorError::IntConversion)
        );
    }
    #[test]
    fn test_tuple_correct() {
        let c: Result<Color, _> = (183, 65, 14).try_into();
        assert!(c.is_ok());
        assert_eq!(
            c.unwrap(),
            Color {
                red: 183,
                green: 65,
                blue: 14
            }
        );
    }
    #[test]
    fn test_array_out_of_range_positive() {
        let c: Result<Color, _> = [1000, 10000, 256].try_into();
        assert_eq!(c, Err(IntoColorError::IntConversion));
    }
    #[test]
    fn test_array_out_of_range_negative() {
        let c: Result<Color, _> = [-10, -256, -1].try_into();
        assert_eq!(c, Err(IntoColorError::IntConversion));
    }
    #[test]
    fn test_array_sum() {
        let c: Result<Color, _> = [-1, 255, 255].try_into();
        assert_eq!(c, Err(IntoColorError::IntConversion));
    }
    #[test]
    fn test_array_correct() {
        let c: Result<Color, _> = [183, 65, 14].try_into();
        assert!(c.is_ok());
        assert_eq!(
            c.unwrap(),
            Color {
                red: 183,
                green: 65,
                blue: 14
            }
        );
    }
    #[test]
    fn test_slice_out_of_range_positive() {
        let arr = [10000, 256, 1000];
        assert_eq!(
            Color::try_from(&arr[..]),
            Err(IntoColorError::IntConversion)
        );
    }
    #[test]
    fn test_slice_out_of_range_negative() {
        let arr = [-256, -1, -10];
        assert_eq!(
            Color::try_from(&arr[..]),
            Err(IntoColorError::IntConversion)
        );
    }
    #[test]
    fn test_slice_sum() {
        let arr = [-1, 255, 255];
        assert_eq!(
            Color::try_from(&arr[..]),
            Err(IntoColorError::IntConversion)
        );
    }
    #[test]
    fn test_slice_correct() {
        let v = vec![183, 65, 14];
        let c: Result<Color, _> = Color::try_from(&v[..]);
        assert!(c.is_ok());
        assert_eq!(
            c.unwrap(),
            Color {
                red: 183,
                green: 65,
                blue: 14
            }
        );
    }
    #[test]
    fn test_slice_excess_length() {
        let v = vec![0, 0, 0, 0];
        assert_eq!(Color::try_from(&v[..]), Err(IntoColorError::BadLen));
    }
    #[test]
    fn test_slice_insufficient_length() {
        let v = vec![0, 0];
        assert_eq!(Color::try_from(&v[..]), Err(IntoColorError::BadLen));
    }
}

```

---

### 本题内容

在这个练习中，你将实现 `TryFrom` 特征，这是 Rust 中用于类型转换的一个特征，允许你在转换可能失败的情况下返回一个错误。这与 `From` 特征类似，不过 `From` 用于那些保证不会失败的转换。`TryFrom` 广泛用于需要验证输入参数并可能因不满足条件而失败的场合。

### 相关知识点

1. **`TryFrom` 和 `TryInto` 特征**：这两个特征用于执行可能失败的类型转换。`TryFrom` 负责定义如何从某种类型转换来，而 `TryInto` 则利用 `TryFrom` 的实现提供了反向转换。
   
2. **错误处理**：实现 `TryFrom` 时，你需要定义一个错误类型来表示转换可能失败的原因。这种模式在 Rust 中用于提供详细的错误信息，增加代码的健壮性。

3. **类型检查和范围验证**：在转换过程中，特别是处理颜色值时，需要检查数值是否在有效范围内（如 RGB 值必须在 0 到 255 之间）。

### 解题方法

步骤描述：
1. **参数验证**：首先检查输入字符串的长度，如果为零或格式不正确（不包含两个逗号分隔的部分），则返回相应的错误。
   
2. **分割字符串**：使用逗号将字符串分割为两部分，分别代表颜色的名字和值。

3. **转换和验证**：将分割后的字符串转换为 `usize`，同时确保这些值在 0 到 255 的范围内。

4. **返回结果或错误**：如果所有验证都通过，则创建 `Color` 结构体并返回；如果任何步骤失败，则返回一个描述问题的错误。

### 代码示例

```rust
use std::convert::TryFrom;

#[derive(Debug, PartialEq)]
struct Color {
    red: u8,
    green: u8,
    blue: u8,
}

#[derive(Debug, PartialEq)]
enum IntoColorError {
    BadLen,
    IntConversion,
}

// 实现 TryFrom for Tuple
impl TryFrom<(i16, i16, i16)> for Color {
    type Error = IntoColorError;

    fn try_from(tuple: (i16, i16, i16)) -> Result<Self, Self::Error> {
        if tuple.0 < 0 || tuple.0 > 255 || tuple.1 < 0 || tuple.1 > 255 || tuple.2 < 0 || tuple.2 > 255 {
            Err(IntoColorError::IntConversion)
        } else {
            Ok(Color {
                red: tuple.0 as u8,
                green: tuple.1 as u8,
                blue: tuple.2 as u8,
            })
        }
    }
}

// 实现 TryFrom for Array
impl TryFrom<[i16; 3]> for Color {
    type Error = IntoColorError;

    fn try_from(arr: [i16; 3]) -> Result<Self, Self::Error> {
        // 使用 tuple 的实现转换 array，简化代码
        Color::try_from((arr[0], arr[1], arr[2]))
    }
}

// 实现 TryFrom for Slice
impl TryFrom<&[i16]> for Color {
    type Error = IntoColorError;

    fn try_from(slice: &[i16]) -> Result<Self, Self::Error> {
        // 首先检查 slice 的长度
        if slice.len() != 3 {
            Err(IntoColorError::BadLen)
        } else {
            // 如果长度正确，尝试将 slice 转换为 array，再调用 array 的实现
            let red = slice[0];
            let green = slice[1];
            let blue = slice[2];
            Color::try_from([red, green, blue])
        }
    }
}

fn main() {
    // 使用 `try_from` function
    let c1 = Color::try_from((183, 65, 14));
    println!("{:?}", c1);

    // Since TryFrom is implemented for Color, we should be able to use TryInto
    let c2: Result<Color, _> = [183, 65, 14].try_into();
    println!("{:?}", c2);

    let v = vec![183, 65, 14];
    // With slice we should use `try_from` function
    let c3 = Color::try_from(&v[..]);
    println!("{:?}", c3);
    // or take slice within round brackets and use TryInto
    let c4: Result<Color, _> = (&v[..]).try_into();
    println!("{:?}", c4);
}

```

这个实现示例展示了如何为不同类型的数据结构提供 `TryFrom` 实现，同时确保所有输入在预期的范围内，并处理任何类型转换错误。

---

## 扩展知识点与解答：

### 扩展知识点：

1. **`TryFrom` 和 `TryInto` 特质**：
   - `TryFrom` 和 `TryInto` 特质允许进行可能失败的类型转换。与 `From` 和 `Into` 不同，它们返回一个 `Result` 类型，允许错误处理和适当的反馈。
   - 这对于处理外部数据或者任何可能失败的转换场景（如用户输入或网络数据解析）特别有用。

2. **错误处理**：
   - 在 Rust 中处理可能的错误是通过返回 `Result` 类型实现的，这使得错误可以在发生后立即被处理或传播。
   - `map_err` 方法用于将一个错误类型映射为另一个，使得错误类型可以统一或更具体化，便于调用者识别和处理。

3. **泛型编程**：
   - 泛型允许代码对多种数据类型进行操作，而不是针对特定类型。在 `TryFrom` 和 `TryInto` 的实现中，可以对多种整数类型使用泛型，以避免代码重复。

### 扩展解题方法：

1. **细分任务**：
   - 在实现 `TryFrom` 时，首先确认输入数据的有效性。例如，检查数字是否在允许的范围内，或确保输入数据的格式正确。
   - 分步实现对元组、数组和切片的支持，这有助于组织代码并逐步处理复杂性。

2. **利用现有特质**：
   - 利用已经存在的 `FromStr` 特质来帮助实现 `TryFrom<&str>`，这种方法可以重用代码并减少出错的机会。

3. **错误传播**：
   - 使用 `?` 操作符简化错误处理。当转换可能失败时（如解析整数），`?` 允许错误自动传播而不需要显式处理。

4. **单元测试**：
   - 为每种可能的错误情况和成功情况编写单元测试。确保代码能正确处理所有预期的输入以及边缘情况，比如输入格式错误、数值越界等。

通过以上的方法，可以确保 `TryFrom` 和 `TryInto` 实现既健壮又易于维护，同时也提供了充分的错误信息，使得调用者可以更容易地理解和处理发生的错误。这些实践不仅限于此任务，也适用于任何需要类型安全转换和错误处理的场景。

## 代码示例：网络请求的 JSON 数据转换

在实践中，我们经常需要处理从一个复杂数据结构到另一个复杂数据结构的转换，这些转换可能会因为数据不符合预期而失败。例如，我们可能需要从 JSON 数据解析得到一个用户定义的复杂对象。在这种情况下，使用 `TryFrom` 和 `TryInto` 特质能够帮助我们更安全地处理潜在的错误。

假设我们有一个表示网络请求的 JSON 数据，并且我们需要将这些数据转换成 Rust 中的结构体以进行进一步处理。以下是一个示例，展示了如何定义和实现这种转换，以及如何处理可能的错误：

```rust
use std::convert::{TryFrom, TryInto};
use serde::{Deserialize, Serialize};
use serde_json::Error as SerdeError;

#[derive(Debug, Deserialize, Serialize)]
struct User {
    id: u32,
    username: String,
    email: String,
}

#[derive(Debug)]
struct ApiRequest {
    endpoint: String,
    body: String,
}

#[derive(Debug)]
enum ApiRequestError {
    JsonError(SerdeError),
    MissingField(String),
}

impl TryFrom<ApiRequest> for User {
    type Error = ApiRequestError;

    fn try_from(request: ApiRequest) -> Result<Self, Self::Error> {
        if request.endpoint != "/users" {
            return Err(ApiRequestError::MissingField("Incorrect endpoint".to_string()));
        }

        serde_json::from_str(&request.body).map_err(ApiRequestError::JsonError)
    }
}

fn main() {
    let request = ApiRequest {
        endpoint: "/users".to_string(),
        body: r#"{"id": 1, "username": "john_doe", "email": "john@example.com"}"#.to_string(),
    };

    match request.try_into() {
        Ok(user) => println!("Received user: {:?}", user),
        Err(e) => println!("Error parsing request: {:?}", e),
    }
}
```

### 关键点分析：

1. **数据结构**：
   - `User`：一个简单的用户信息结构体，包括用户 ID、用户名和电子邮件。
   - `ApiRequest`：表示 API 请求的数据结构，包括请求端点和请求体（JSON 格式的字符串）。

2. **错误处理**：
   - `ApiRequestError`：自定义错误类型，用于处理从 `ApiRequest` 到 `User` 的转换中可能出现的错误，如 JSON 解析错误和缺少必要的字段。

3. **`TryFrom` 的实现**：
   - 在尝试从 `ApiRequest` 转换为 `User` 时，首先检查端点是否正确，然后尝试解析 JSON 字符串。
   - 使用 `serde_json::from_str` 进行 JSON 解析，并通过 `map_err` 方法将 `SerdeError` 转换为 `ApiRequestError`。

这个示例展示了如何安全地处理可能失败的类型转换，并通过自定义错误类型提供更多关于失败原因的信息，使得错误处理更为清晰和有用。
