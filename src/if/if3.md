# Exercise 15

- Name: ```if3```
- Path: ```exercises/if/if3.rs```
#### Hint: 

In Rust, every arm of an `if` expression has to return the same type of value. Make sure the type is consistent across all arms.


---



```rust,editable
// if3.rs
//
// Execute `rustlings hint if3` or use the `hint` watch subcommand for a hint.

// I AM NOT DONE

pub fn animal_habitat(animal: &str) -> &'static str {
    let identifier = if animal == "crab" {
        1
    } else if animal == "gopher" {
        2.0
    } else if animal == "snake" {
        3
    } else {
        "Unknown"
    };

    // DO NOT CHANGE THIS STATEMENT BELOW
    let habitat = if identifier == 1 {
        "Beach"
    } else if identifier == 2 {
        "Burrow"
    } else if identifier == 3 {
        "Desert"
    } else {
        "Unknown"
    };

    habitat
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn gopher_lives_in_burrow() {
        assert_eq!(animal_habitat("gopher"), "Burrow")
    }

    #[test]
    fn snake_lives_in_desert() {
        assert_eq!(animal_habitat("snake"), "Desert")
    }

    #[test]
    fn crab_lives_on_beach() {
        assert_eq!(animal_habitat("crab"), "Beach")
    }

    #[test]
    fn unknown_animal() {
        assert_eq!(animal_habitat("dinosaur"), "Unknown")
    }
}

```

---

### 本题内容：

这个练习关注于保证`if`表达式的所有分支返回相同类型的值。题目中的代码尝试通过不同的分支返回不同类型的值给变量`identifier`，这违反了Rust的类型一致性规则。

### 相关知识点：

- **类型一致性**：Rust要求`if`表达式的所有分支返回相同的类型。这是因为`if`表达式的值需要在编译时可预测，以便进行类型检查和推断。
- **静态生命周期引用**：返回字符串字面值作为引用时，它们的类型是`&'static str`，这表明这些字符串在程序的整个生命周期内都是有效的。

### 解题方法：

- **步骤描述**：
  - 修正代码，确保所有`if`分支返回相同类型的值。考虑到最终的目标是返回一个表示动物栖息地的字符串，我们可以直接在`if`表达式中返回字符串字面值，而无需通过中间的数值标识符。

- **代码示例**：
    

```rust
// if3.rs
// 已完成练习

pub fn animal_habitat(animal: &str) -> &'static str {
    if animal == "crab" {
        "Beach"
    } else if animal == "gopher" {
        "Burrow"
    } else if animal == "snake" {
        "Desert"
    } else {
        "Unknown"
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn gopher_lives_in_burrow() {
        assert_eq!(animal_habitat("gopher"), "Burrow");
    }

    #[test]
    fn snake_lives_in_desert() {
        assert_eq!(animal_habitat("snake"), "Desert");
    }

    #[test]
    fn crab_lives_on_beach() {
        assert_eq!(animal_habitat("crab"), "Beach");
    }

    #[test]
    fn unknown_animal() {
        assert_eq!(animal_habitat("dinosaur"), "Unknown");
    }
}
```
在这个修正后的版本中，`animal_habitat`函数直接根据`animal`的值返回对应的栖息地字符串。所有`if`分支都返回`&'static str`类型的值，满足Rust的类型一致性要求。

通过完成这个练习，你将学会如何处理Rust中`if`表达式要求所有分支返回相同类型值的情况，这对于编写类型安全的Rust代码非常重要。

## 扩展知识点与解答：

探索`if3`练习之后，我们可以进一步理解Rust中`if`表达式的灵活性以及如何有效地利用单元测试来验证代码的正确性。

### 扩展知识点：

1. **`if`表达式的灵活性**：
   - Rust中的`if`表达式不仅能够根据条件执行不同的代码块，而且能够返回值，这使得它们在许多场景下都非常有用。利用这一点，可以简化代码，避免使用额外的变量。

2. **生命周期注解**：
   - 当函数返回引用时，Rust可能需要生命周期注解来确保引用的有效性。在本题中，所有返回的字符串字面值都具有`'static`生命周期，意味着它们在整个程序运行期间都是有效的，因此不需要额外的生命周期注解。

3. **单元测试的作用**：
   - 单元测试是验证函数或模块行为是否符合预期的有效方法。在Rust中，单元测试通常位于与实现代码相同的文件中，并通过`#[cfg(test)]`属性标注的模块进行组织。

### 扩展解题方法：

- **优化`if`表达式使用**：
  - 在实践中，考虑使用`match`表达式重构复杂的`if-else if`链，特别是当你需要基于同一个表达式的不同结果执行不同操作时。

- **代码示例见`if2`**

### 测试代码详解：

- Rust的单元测试通过`#[test]`属性标记测试函数。`assert_eq!`宏用来比较期望值和实际值，如果不相等，则测试失败。使用单元测试可以帮助开发者确保代码更改不会破坏现有功能。

- **代码示例（单元测试）**：

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn gopher_lives_in_burrow() {
        // 验证函数对特定输入返回正确的输出
        assert_eq!(animal_habitat("gopher"), "Burrow");
    }

    // 其他测试省略...
}
```
在这个示例中，每个测试函数检查`animal_habitat`函数对于特定输入是否返回预期的输出。如果函数的行为发生变化，这些测试将帮助迅速识别问题。
