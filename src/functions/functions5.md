# Exercise 12

- Name: ```functions5```
- Path: ```exercises/functions/functions5.rs```
#### Hint: 

This is a really common error that can be fixed by removing one character.

It happens because Rust distinguishes between expressions and statements: expressions return a value based on their operand(s), and statements simply return a () type which behaves just like `void` in C/C++ language.

We want to return a value of `i32` type from the `square` function, but it is returning a `()` type...
They are not the same. There are two solutions:

1. Add a `return` ahead of `num * num;`
2. remove `;`, make it to be `num * num`


---



```rust,editable
// functions5.rs
//
// Execute `rustlings hint functions5` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

fn main() {
    let answer = square(3);
    println!("The square of 3 is {}", answer);
}

fn square(num: i32) -> i32 {
    num * num;
}

```

---

### 本题内容：

这个练习旨在解释Rust中表达式和语句的区别，以及它们如何影响函数的返回值。特别地，它展示了如何通过简单地调整函数体的结尾来正确返回一个值，而不是默认的`()`类型。

### 相关知识点：

- **表达式 vs 语句**：在Rust中，表达式是计算并产生一个值的代码片段，而语句则是执行某些操作但不返回值的代码片段。函数体中的最后一个表达式可以隐式作为函数的返回值，前提是它后面没有分号。
- **返回值**：Rust函数默认返回最后一个表达式的值，除非该表达式后跟有分号，这会将其转换为语句，导致函数返回`()`类型。

### 解题方法：

- **步骤描述**：
  - 为了让`square`函数返回`num * num`的计算结果，需要确保这个计算是函数体中的最后一个表达式，并且后面没有分号。这样，它就不会被视为语句，而是一个值返回表达式。

- **代码示例**：
    ```rust
    // functions5.rs
    // 已完成练习
    
    fn main() {
        let answer = square(3);
        println!("The square of 3 is {}", answer);
    }
    
    fn square(num: i32) -> i32 {
        num * num // 移除了分号，使其成为一个返回值的表达式
    }
    ```
    在这个修正后的版本中，通过移除`num * num;`后面的分号，`square`函数现在能正确返回计算结果。因此，当`main`函数调用`square(3)`时，它将计算3的平方并返回结果9，然后打印出"The square of 3 is 9"。这个练习展示了如何在Rust函数中返回一个计算值，以及表达式和语句之间细微但重要的区别。

## 扩展知识点与解答：

探索`functions5`练习之后，我们可以深入了解Rust中表达式和语句的概念，以及如何通过这些概念编写更灵活和强大的函数。

### 扩展知识点：

1. **隐式返回值**：
   - Rust允许函数隐式返回其最后一个表达式的值。这是通过省略`return`关键字和最后一个表达式后面的分号来实现的。这种特性使得函数定义更加简洁，尤其是在函数体较短时。

2. **`return`关键字**：
   - 尽管Rust支持隐式返回值，但在需要提前退出函数或者在函数的非末尾位置返回值时，`return`关键字仍然非常有用。使用`return`可以明确地从任何地方返回函数的值。

3. **表达式的用途**：
   - 几乎所有的Rust代码结构都是表达式，包括块`{}`、条件表达式`if`、循环`loop`等。这意味着它们都可以计算出一个值，为Rust提供了极高的表达力和灵活性。

### 扩展解题方法：

- **优化函数返回**：
  - 在实践中，优先考虑使用隐式返回值，使函数体更简洁。但在逻辑复杂，需要从多个分支返回值的情况下，适当使用`return`关键字来提早退出函数。

- **利用表达式的特性**：
  - 通过理解Rust中表达式和语句的区别，可以更好地控制函数的返回值和侧效应，编写出更精炼和强大的逻辑。

- **代码示例（使用`if`表达式返回值）**：
    ```rust
    fn bigger(a: i32, b: i32) -> i32 {
        if a > b { a } else { b }
    }
    
    fn main() {
        println!("Bigger number is: {}", bigger(42, 105));
    }
    ```
    这个示例中，`bigger`函数利用`if`表达式的特性来决定并返回两个数中较大的一个。这里`if`表达式的结果直接作为函数的返回值，展示了Rust中表达式强大的表达能力。
