# Exercise 73

- Name: ```iterators2```
- Path: ```exercises/iterators/iterators2.rs```
#### Hint: 

Step 1

The variable `first` is a `char`. It needs to be capitalized and added to the remaining characters in `c` in order to return the correct `String`. The remaining characters in `c` can be viewed as a string slice using the
`as_str` method.

The documentation for `char` contains many useful methods.

[https://doc.rust-lang.org/std/primitive.char.html ](https://doc.rust-lang.org/std/primitive.char.html) ([Chinese version](https://rustwiki.org/zh-CN/std/primitive.char.html))

Step 2

Create an iterator from the slice. Transform the iterated values by applying the `capitalize_first` function. Remember to collect the iterator.

Step 3.

This is surprisingly similar to the previous solution. Collect is very powerful and very general. Rust just needs to know the desired type.


---



```rust,editable
// iterators2.rs
//
// In this exercise, you'll learn some of the unique advantages that iterators
// can offer. Follow the steps to complete the exercise.
//
// Execute `rustlings hint iterators2` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

// Step 1.
// Complete the `capitalize_first` function.
// "hello" -> "Hello"
pub fn capitalize_first(input: &str) -> String {
    let mut c = input.chars();
    match c.next() {
        None => String::new(),
        Some(first) => ???,
    }
}

// Step 2.
// Apply the `capitalize_first` function to a slice of string slices.
// Return a vector of strings.
// ["hello", "world"] -> ["Hello", "World"]
pub fn capitalize_words_vector(words: &[&str]) -> Vec<String> {
    vec![]
}

// Step 3.
// Apply the `capitalize_first` function again to a slice of string slices.
// Return a single string.
// ["hello", " ", "world"] -> "Hello World"
pub fn capitalize_words_string(words: &[&str]) -> String {
    String::new()
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_success() {
        assert_eq!(capitalize_first("hello"), "Hello");
    }

    #[test]
    fn test_empty() {
        assert_eq!(capitalize_first(""), "");
    }

    #[test]
    fn test_iterate_string_vec() {
        let words = vec!["hello", "world"];
        assert_eq!(capitalize_words_vector(&words), ["Hello", "World"]);
    }

    #[test]
    fn test_iterate_into_string() {
        let words = vec!["hello", " ", "world"];
        assert_eq!(capitalize_words_string(&words), "Hello World");
    }
}

```

---

### 本题内容

在这个练习中，我们将通过完成一个涉及字符串处理的任务来探索迭代器的独特优势。具体任务是实现字符串首字母大写的功能，并将其应用于字符串数组，从而熟悉迭代器的基本操作和数据转换。

### 解题方法

#### 步骤描述：

**步骤 1：完成 `capitalize_first` 函数**
- 首先，通过 `input.chars()` 创建一个字符迭代器。
- 使用 `next()` 方法获取第一个字符，如果存在，将其转换为大写，然后与剩余字符拼接。
- 如果输入字符串为空，则返回一个空字符串。

**步骤 2：实现 `capitalize_words_vector` 函数**
- 使用 `map` 函数将 `capitalize_first` 应用于每个字符串。
- 使用 `collect()` 方法将处理后的迭代器转换回 `Vec<String>`。

**步骤 3：实现 `capitalize_words_string` 函数**
- 与步骤 2 类似，但结果需要拼接成一个单独的字符串。
- 使用 `collect::<String>()` 方法将字符拼接成一个完整的字符串。

#### 代码示例：

```rust
// Step 1: 完成首字母大写的函数
pub fn capitalize_first(input: &str) -> String {
    let mut c = input.chars();
    match c.next() {
        None => String::new(),
        Some(first) => {
            let mut capitalized = first.to_uppercase().to_string();  // 转换第一个字符为大写
            capitalized.push_str(c.as_str());  // 添加剩余字符
            capitalized
        },
    }
}

// Step 2: 应用首字母大写函数到字符串数组，返回字符串数组
pub fn capitalize_words_vector(words: &[&str]) -> Vec<String> {
    words.iter().map(|word| capitalize_first(word)).collect()
}

// Step 3: 应用首字母大写函数到字符串数组，返回单一字符串
pub fn capitalize_words_string(words: &[&str]) -> String {
    words.iter().map(|word| capitalize_first(word)).collect::<String>()
}
```

通过上述步骤和代码示例，可以看到如何通过迭代器以及 Rust 的字符串处理能力来解决实际问题，提高数据处理的灵活性和效率。这个练习也展示了如何结合使用迭代器的各种方法来处理更复杂的数据结构和操作。

## 扩展知识点与解答：

### 扩展知识点

1. **链式方法调用**：
   - Rust 中的迭代器支持链式调用，允许你将多个迭代器方法顺序地链接在一起，这样可以以非常高的效率和简洁的代码完成复杂的数据处理。

2. **字符串和字符处理**：
   - Rust 的标准库提供了强大的工具来处理字符串和字符，例如 `.to_uppercase()` 可以将字符转换为大写，而这在其他编程语言中可能需要更多的操作。

3. **迭代器性能优势**：
   - 使用迭代器可以提高代码的性能，因为迭代器通常是惰性计算的，即它们只在需要时才计算值。这种方式可以避免不必要的内存使用和计算开销。

4. **更多的迭代器方法**：
   - 除了 `map` 和 `collect`，Rust 迭代器还有很多其他有用的方法如 `filter`, `fold`, `reduce`, `zip`, `chain` 等，这些都是处理集合和序列的强大工具。

### 扩展解题方法

1. **更复杂的字符串操作**：
   - 可以扩展当前的函数以处理更复杂的字符串操作，如处理包含多种语言字符的字符串，使用 Unicode 标准进行正确的大小写转换等。

2. **组合不同的迭代器方法**：
   - 将 `map` 与其他迭代器方法组合，如使用 `filter` 来只选择符合特定条件的元素进行首字母大写处理。例如，仅对长度大于3的单词应用首字母大写。

3. **错误处理和更健壮的代码**：
   - 在转换过程中加入错误处理，确保例如空字符串或特殊字符不会导致程序崩溃。可以使用 `Result` 类型来封装可能的错误。

4. **使用迭代器适配器进一步优化代码**：
   - 利用如 `take`, `skip` 等迭代器适配器来处理只需要部分数据的情况，提高代码的执行效率和适应性。

#### 示例：结合 `filter` 和 `map` 处理特定单词

```rust
pub fn capitalize_long_words(words: &[&str]) -> Vec<String> {
    words.iter()
         .filter(|&word| word.len() > 3)
         .map(|word| capitalize_first(word))
         .collect()
}
```

这段代码只对长度大于3的单词进行首字母大写处理，展示了如何通过组合迭代器方法来解决更具体的问题。通过这样的扩展学习和应用，可以更深入地理解迭代器的强大功能和灵活性。
