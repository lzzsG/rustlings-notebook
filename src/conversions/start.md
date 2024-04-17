# conversions

---

本章节我们将涵盖一系列练习，旨在深化你对 Rust 中类型转换和引用转换特性的理解和应用。

### Exercise 91: 使用 `as` 运算符

这个练习聚焦于 `as` 运算符的使用，它是 Rust 中进行显式类型转换的基本工具。通过这个练习，你将学习如何在函数中正确使用 `as` 运算符来保证数值操作的类型安全，特别是在除法等操作中确保结果类型的正确性。

### Exercise 92: `From` 和 `Into` 特性

接下来的练习将引导你探索 `From` 和 `Into` 特性。这些特性用于实现类型之间的转换，使得代码更加灵活且易于维护。你将具体实现如何将字符串解析为自定义数据类型 `Person`，涉及字符串拆分、解析以及错误处理。

### Exercise 93: 实现 `FromStr` 特性

此练习要求你实现 `FromStr` 特性，它用于从字符串解析数据时提供详细的错误处理。与 `From` 特性不同，`FromStr` 允许返回错误，这对于处理外部输入尤为重要。通过这个练习，你将学习如何在解析过程中捕获并处理各种潜在错误。

### Exercise 94: `TryFrom` 和 `TryInto` 特性

`TryFrom` 和 `TryInto` 特性扩展了 `From` 和 `Into` 的功能，允许在转换可能失败时返回 `Result` 类型，从而提供了一种安全的类型转换方式。你将通过实现这些特性来控制数据转换过程中的错误，并确保程序的鲁棒性。

### Exercise 95: `AsRef` 和 `AsMut` 特性

最后，你将研究 `AsRef` 和 `AsMut` 特性，它们提供了一种将值转换为引用或可变引用的方式。这些特性在需要处理不同类型但又不希望获取所有权时特别有用。通过这个练习，你将学习如何有效地使用这些特性来访问和修改数据，而无需进行昂贵的拷贝操作。

每个练习都设计来帮助你理解和应用 Rust 中的不同转换特性，从而写出更安全、更高效、更清晰的 Rust 代码。

---

```rust
// 你不必现在理解以下代码，不过你可以尝试运行它。

use std::str::FromStr;
use std::convert::{TryFrom, TryInto, AsRef};

fn safe_division(dividend: i32, divisor: i32) -> i32 {
    if divisor == 0 {
        panic!("Attempted to divide by zero");
    }
    (dividend as f32 / divisor as f32) as i32
}

#[derive(Debug)]
struct Person {
    name: String,
    age: u32,
}

impl From<&str> for Person {
    fn from(s: &str) -> Self {
        let parts: Vec<&str> = s.split(',').collect();
        if parts.len() != 2 {
            panic!("Invalid input");
        }
        Person {
            name: parts[0].to_string(),
            age: parts[1].trim().parse().expect("Invalid age"),
        }
    }
}

impl FromStr for Person {
    type Err = String;

    fn from_str(s: &str) -> Result<Self, Self::Err> {
        let parts: Vec<&str> = s.split(',').collect();
        if parts.len() != 2 {
            return Err("Input does not contain exactly two parts".to_string());
        }
        let age = parts[1].trim().parse::<u32>();
        match age {
            Ok(age) => Ok(Person {
                name: parts[0].to_string(),
                age,
            }),
            Err(_) => Err("Second part is not a valid age".to_string()),
        }
    }
}

struct SafeNumber(i32);

impl TryFrom<i32> for SafeNumber {
    type Error = String;

    fn try_from(value: i32) -> Result<Self, Self::Error> {
        if value > 0 {
            Ok(SafeNumber(value))
        } else {
            Err("Value must be positive".to_string())
        }
    }
}

fn print_length<T: AsRef<str>>(input: T) {
    let input_ref = input.as_ref();
    println!("The length of '{}' is {}.", input_ref, input_ref.len());
}

fn main() {
    println!("Welcome to the depths of the dark forest!");

    let result = safe_division(10, 3);
    println!("Safe division result is: {}", result);

    let person = Person::from("Ferris,18");
    println!("Created a mystical person ID: {:?}", person);

    match Person::from_str("Gandalf,1000") {
        Ok(wizard) => println!("You have successfully parsed a wizard: {:?}", wizard),
        Err(e) => println!("Failed to parse wizard: {}", e),
    }

    match SafeNumber::try_from(-10) {
        Ok(_) => println!("Successfully created a safe number."),
        Err(e) => println!("Failed to create a safe number: {}", e),
    }

    print_length("A strange inscription on an ancient tree.");

    println!("\nYour🌲journey🌲continues🌲into🌲the🌲dark🌲forest🌲\nA thick fog envelops you, dampening sound and distorting shapes, making each step \nforward an act of faith.");
}
```

