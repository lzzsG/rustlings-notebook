# Exercise 27

- Name: ```move_semantics3```
- Path: ```exercises/move_semantics/move_semantics3.rs```
#### Hint: 

The difference between this one and the previous ones is that the first line of `fn fill_vec` that had `let mut vec = vec;` is no longer there.

You can, instead of adding that line back, add `mut` in one place that will change an existing binding to be a mutable binding instead of an immutable one :)


---



```rust,editable
// move_semantics3.rs
//
// Make me compile without adding new lines-- just changing existing lines! (no
// lines with multiple semicolons necessary!)
//
// Execute `rustlings hint move_semantics3` or use the `hint` watch subcommand
// for a hint.

// I AM NOT DONE

fn main() {
    let vec0 = Vec::new();

    let mut vec1 = fill_vec(vec0);

    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);

    vec1.push(88);

    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);
}

fn fill_vec(vec: Vec<i32>) -> Vec<i32> {
    vec.push(22);
    vec.push(44);
    vec.push(66);

    vec
}

```

### 本题内容：

这个练习要求通过修改现有代码行，而不是增加新行，来解决编译错误。目标是修改`fill_vec`函数，使其能够在不重新声明变量的情况下修改传入的向量`vec`。

### 相关知识点：

- **可变性关键字`mut`**：Rust中的变量默认是不可变的。要修改变量，需要在声明时使用`mut`关键字使其可变。

- **函数参数的可变性**：当函数需要修改传入的参数时，参数必须被声明为可变。这通过在参数前添加`mut`关键字来实现。

- **所有权与函数**：在Rust中，将变量作为参数传递给函数时，默认会发生所有权的转移。如果函数需要保持参数的可变性，参数类型应声明为可变。

### 解题方法：

#### 修改参数为可变
- **步骤描述**：
  1. 在`fill_vec`函数的参数声明中添加`mut`关键字，使`vec`参数变为可变。这允许函数内部对`vec`进行修改。
  2. 因为`vec`参数现在是可变的，你可以直接在其上调用`.push()`方法添加元素，而无需先将其赋值给另一个可变变量。

#### 代码示例：
```rust
fn main() {
    let vec0 = Vec::new();

    let mut vec1 = fill_vec(vec0); // vec0 的所有权被转移给 fill_vec

    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);

    vec1.push(88);

    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);
}

// 修改函数参数，使其可变
fn fill_vec(mut vec: Vec<i32>) -> Vec<i32> {
    vec.push(22);
    vec.push(44);
    vec.push(66);

    vec
}
```

通过这个简单的修改，你可以解决编译错误并使程序正常运行。这个练习突出了Rust中可变性和所有权的重要性，以及如何通过细微的修改来改变程序的行为。掌握这些概念对于理解Rust的内存安全保证和编写有效的Rust代码至关重要。
