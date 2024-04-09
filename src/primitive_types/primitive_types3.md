# Exercise 19

- Name: ```primitive_types3```
- Path: ```exercises/primitive_types/primitive_types3.rs```
#### Hint: 

There's a shorthand to initialize Arrays with a certain size that does not require you to type in 100 items (but you certainly can if you want!).

For example, you can do: `let array = ["Are we there yet?"; 10];`

Bonus: what are some other things you could have that would return true for `a.len() >= 100`?


---



```rust,editable
// primitive_types3.rs
//
// Create an array with at least 100 elements in it where the ??? is.
//
// Execute `rustlings hint primitive_types3` or use the `hint` watch subcommand
// for a hint.

// I AM NOT DONE

fn main() {
    let a = ???

    if a.len() >= 100 {
        println!("Wow, that's a big array!");
    } else {
        println!("Meh, I eat arrays like that for breakfast.");
    }
}

```

---

### 本题内容：

这个练习旨在让学生熟悉Rust中数组的声明和初始化，特别是初始化具有大量元素的数组的简便方法。通过完成这个任务，学生将学习如何高效地创建和使用数组。

### 相关知识点：

- **数组**：Rust中的数组是一个固定大小的集合，其中包含了相同类型的元素。数组在声明时其大小必须是已知的，且一旦声明，其大小不能改变。
- **数组初始化**：Rust提供了几种方法来初始化数组。对于具有大量元素的数组，可以使用语法`[value; size]`来创建一个所有元素都被设置为`value`，且大小为`size`的数组。
- **数组长度**：使用`.len()`方法可以获取数组的长度。

### 解题方法：

- **步骤描述**：
  1. 使用提供的示例语法，创建一个包含至少100个元素的数组。你可以选择任何类型的元素，但所有元素必须是相同的类型。
  2. 确保数组的长度符合`if`语句中的条件，以打印出“Wow, that's a big array!”的消息。

- **代码示例**：
    

```rust
// primitive_types3.rs
// 已完成练习

fn main() {
    // 创建一个包含100个元素的数组，每个元素都是字符串"element"
    let a = ["element"; 100];

    if a.len() >= 100 {
        println!("Wow, that's a big array!");
    } else {
        println!("Meh, I eat arrays like that for breakfast.");
    }
}
```
在这个修正后的版本中，我们创建了一个包含100个字符串"element"的数组`a`。这种数组初始化的简写方法大大简化了代码，尤其是当需要初始化大量元素时。

## 扩展知识点与解答：

在完成`primitive_types3`练习之后，我们不仅学会了如何高效地声明和初始化具有大量元素的数组，还揭开了Rust中处理集合类型数据的更广泛视角。以下是一些相关的扩展知识点和解题方法，这些内容将帮助你更深入地理解Rust中的集合类型及其应用。

### 扩展知识点：

1. **向量(`Vec<T>`)**：
   - 向量是Rust中一个动态数组，可以在运行时增加或删除元素。与数组不同，向量的大小不需要在编译时确定，使其成为处理可变大小集合的理想选择。

2. **字符串(`String`和`str`)**：
   - 在Rust中，字符串有两种主要形式：`String`和`str`。`String`是一个可增长的、堆分配的字符串类型，而`str`通常以字符串切片(`&str`)的形式出现，表示一个固定大小的字符串。

3. **切片(`slice`)**：
   - 切片是对集合部分元素的引用，允许你访问数组或向量的一段连续元素而不拥有它们。切片在处理部分数据时非常有用。

### 扩展解题方法：

- **探索向量操作**：
  - 练习使用向量并探索其提供的丰富API，如`push`、`pop`等方法，以及如何利用迭代器对向量进行遍历和处理。

- **字符串处理**：
  - 尝试创建和修改`String`类型的变量，了解如何将`String`和`&str`之间进行转换，以及如何拼接和切分字符串。

- **使用切片**：
  - 练习创建和使用切片，了解如何通过切片访问和修改数组或向量的部分元素。

- **代码示例（向量和字符串操作）**：
    

```rust
fn main() {
    let mut vec = vec![1, 2, 3];
    vec.push(4);
    println!("Vector: {:?}", vec);

    let mut s = String::from("Hello, ");
    s.push_str("world!");
    println!("String: {}", s);

    let slice = &vec[1..3];
    println!("Slice: {:?}", slice);
}
```
这个示例展示了如何声明和操作向量、字符串以及切片。通过向量添加元素、拼接字符串和访问向量的切片，我们可以看到Rust在处理动态集合数据方面的灵活性和强大功能
