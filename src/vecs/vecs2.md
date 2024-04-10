# Exercise 24

- Name: ```vecs2```
- Path: ```exercises/vecs/vecs2.rs```
#### Hint: 

Hint 1: In the code, the variable `element` represents an item from the Vec as it is being iterated.
Can you try multiplying this?

Hint 2: For the first function, there's a way to directly access the numbers stored
in the Vec, using the * dereference operator. You can both access and write to the
number that way.

After you've completed both functions, decide for yourself which approach you like
better. What do you think is the more commonly used pattern under Rust developers?



---



```rust,editable
// vecs2.rs
//
// A Vec of even numbers is given. Your task is to complete the loop so that
// each number in the Vec is multiplied by 2.
//
// Make me pass the test!
//
// Execute `rustlings hint vecs2` or use the `hint` watch subcommand for a hint.

// I AM NOT DONE

fn vec_loop(mut v: Vec<i32>) -> Vec<i32> {
    for element in v.iter_mut() {
        // TODO: Fill this up so that each element in the Vec `v` is
        // multiplied by 2.
        ???
    }

    // At this point, `v` should be equal to [4, 8, 12, 16, 20].
    v
}

fn vec_map(v: &Vec<i32>) -> Vec<i32> {
    v.iter().map(|element| {
        // TODO: Do the same thing as above - but instead of mutating the
        // Vec, you can just return the new number!
        ???
    }).collect()
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_vec_loop() {
        let v: Vec<i32> = (1..).filter(|x| x % 2 == 0).take(5).collect();
        let ans = vec_loop(v.clone());

        assert_eq!(ans, v.iter().map(|x| x * 2).collect::<Vec<i32>>());
    }

    #[test]
    fn test_vec_map() {
        let v: Vec<i32> = (1..).filter(|x| x % 2 == 0).take(5).collect();
        let ans = vec_map(&v);

        assert_eq!(ans, v.iter().map(|x| x * 2).collect::<Vec<i32>>());
    }
}

```

---

### 本题内容：

这个练习旨在教会学生如何对向量（`Vec<i32>`）中的每个元素执行操作。具体来说，是将向量中的每个元素乘以2。练习包含两个函数：`vec_loop`和`vec_map`，分别演示了如何通过循环以及通过`map`方法来实现这一操作。

### 相关知识点：

- **可变向量迭代**：使用`.iter_mut()`方法可以获得向量的可变迭代器，允许在迭代过程中修改每个元素的值。
  
- **向量映射**：`.map()`方法允许对向量中的每个元素应用一个函数，并收集结果到一个新的向量中。这是一种函数式编程技巧，用于数据转换。

- **解引用运算符（`*`）**：在可变迭代器中，使用解引用运算符可以访问和修改元素的值。

### 解题方法：

#### 方法一：使用循环和解引用修改元素
- **步骤描述**：
  1. 使用`.iter_mut()`方法获取向量`v`的可变迭代器。
  2. 在循环中，使用解引用运算符`*`修改每个元素的值，将其乘以2。

- **代码示例**：
  

```rust
fn vec_loop(mut v: Vec<i32>) -> Vec<i32> {
    for element in v.iter_mut() {
        *element *= 2; // 解引用并乘以2
    }
    v
}
```

#### 方法二：使用`map`方法创建新向量
- **步骤描述**：
  1. 使用`.iter()`方法获取向量`v`的迭代器。
  2. 应用`.map()`方法，其中每个元素乘以2，并使用`.collect()`将结果收集到一个新向量中。
- **代码示例**：

```rust
fn vec_map(v: &Vec<i32>) -> Vec<i32> {
    v.iter().map(|element| element * 2).collect() // 映射并收集
}
```

## 扩展知识点与解答：

通过解决`vecs2`练习，你不仅练习了如何对向量中的元素进行操作，还接触到了Rust中两种常见的数据处理模式：直接修改数据和使用函数式编程风格生成新的数据集。这些模式不仅适用于向量，还适用于Rust中的其他集合类型。以下是一些相关的扩展知识点和进阶技巧，以及如何在实际编程中应用这些概念。

### 扩展知识点：

1. **所有权和借用**：
   - 在修改向量时，你会直接操作原始数据（通过可变借用）。这要求你理解Rust的所有权和借用规则，尤其是可变借用的规则，以保证代码的安全性和正确性。

2. **迭代器效率**：
   - 迭代器是Rust中处理集合数据的高效方式。Rust的迭代器是惰性的，这意味着它们只在需要产生新值时才会计算，这可以带来性能优势，尤其是在链式调用多个迭代器适配器时。

3. **模式匹配**：
   - 在某些复杂的数据转换场景中，可以结合使用模式匹配和迭代器，以更灵活地处理各种可能的情况。

### 扩展解题方法：

- **性能考虑**：
  - 当处理大型数据集时，选择合适的数据处理模式（直接修改还是生成新集合）可能会对性能产生影响。通常，直接修改原始数据会更高效，但这可能不总是可行或安全的。

- **测试和验证**：
  - 使用单元测试来验证函数的正确性是Rust开发中的常见实践。在`vecs2`练习中，通过比较函数的输出和预期的结果，可以确保函数按预期工作。


### 测试部分解释：

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_vec_loop() {
        let v: Vec<i32> = (1..).filter(|x| x % 2 == 0).take(5).collect();
        let ans = vec_loop(v.clone());
        // 验证vec_loop函数的结果是否符合预期
        assert_eq!(ans, v.iter().map(|x| x * 2).collect::<Vec<i32>>());
    }

    #[test]
    fn test_vec_map() {
        let v: Vec<i32> = (1..).filter(|x| x % 2 == 0).take(5).collect();
        let ans = vec_map(&v);
        // 验证vec_map函数的结果是否符合预期
        assert_eq!(ans, v.iter().map(|x| x * 2).collect::<Vec<i32>>());
    }
}
```
这些测试通过构建向量`v`，然后分别用`vec_loop`和`vec_map`函数处理，最后验证处理结果是否符合预期（即向量`v`中的每个元素乘以2），来确保函数的正确性。

## 继续扩展：

在解决`vecs2`练习的过程中，我们不仅接触到了向量的基本操作，还初步体验了Rust中几个强大的编程概念，包括迭代器、闭包以及迭代器适配器（如`.iter`、`.map`、`.filter`、`.take`、`.collect`等）。这些概念是Rust高效数据处理能力的核心，下面我们将对它们进行更详细的解释，并展望在**后续学习**中如何深入探索这些强大的工具。

### 迭代器（Iterators）

迭代器是Rust中一个用于遍历集合（如向量、哈希表等）中元素的机制。它是惰性的，这意味着迭代器中的计算只有在需要时才会执行。这种设计既提高了效率，也增加了使用上的灵活性。

- **`.iter()`**：创建一个不可变的迭代器，允许你遍历集合而不改变它。
- **`.iter_mut()`**：创建一个可变迭代器，允许你在遍历时修改集合中的元素。

### 闭包（Closures）

闭包是可以捕获其周围作用域变量的匿名函数。在Rust中，闭包广泛用于迭代器适配器中，例如`.map()`或`.filter()`，允许进行强大且灵活的数据处理。

### 迭代器适配器

迭代器适配器允许你对迭代器进行转换，实现更复杂的迭代逻辑。

- **`.map()`**：接受一个闭包，将闭包应用到迭代器中的每个元素上，并返回一个新的迭代器，包含闭包应用后的结果。
- **`.filter()`**：接受一个闭包，闭包返回布尔值，用于决定是否保留元素。
- **`.take()`**：从迭代器中取出前n个元素，生成一个新的迭代器。
- **`.collect()`**：将迭代器中的元素收集到一个集合中，如向量或哈希表。

### 后续学习路径

- **深入迭代器**：学习更多关于迭代器的知识，包括如何使用迭代器适配器进行复杂的数据转换和处理。
- **闭包高级用法**：探索闭包在Rust中的高级用法，包括闭包如何捕获环境中的变量，以及它们在并发编程中的应用。
- **高效数据处理**：了解如何结合迭代器和闭包，编写高效且易于维护的数据处理逻辑。
- **Rust中的函数式编程**：Rust支持函数式编程范式，通过学习和实践，可以更好地利用Rust提供的函数式编程特性，编写简洁且高效的代码。
