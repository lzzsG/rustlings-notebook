# Exercise 44

- Name: ```hashmaps1```
- Path: ```exercises/hashmaps/hashmaps1.rs```
#### Hint: 

Hint 1: Take a look at the return type of the function to figure out  the type for the `basket`.

Hint 2: Number of fruits should be at least 5. And you have to put at least three different types of fruits.

---

```rust,editable
// hashmaps1.rs
//
// A basket of fruits in the form of a hash map needs to be defined. The key
// represents the name of the fruit and the value represents how many of that
// particular fruit is in the basket. You have to put at least three different
// types of fruits (e.g apple, banana, mango) in the basket and the total count
// of all the fruits should be at least five.
//
// Make me compile and pass the tests!
//
// Execute `rustlings hint hashmaps1` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

use std::collections::HashMap;

fn fruit_basket() -> HashMap<String, u32> {
    let mut basket = // TODO: declare your hash map here.

    // Two bananas are already given for you :)
    basket.insert(String::from("banana"), 2);

    // TODO: Put more fruits in your basket here.

    basket
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn at_least_three_types_of_fruits() {
        let basket = fruit_basket();
        assert!(basket.len() >= 3);
    }

    #[test]
    fn at_least_five_fruits() {
        let basket = fruit_basket();
        assert!(basket.values().sum::<u32>() >= 5);
    }
}

```

---

### 本题内容：

这个练习要求你创建一个表示水果篮子的`HashMap`。其中，键（Key）表示水果的名称，值（Value）表示篮子中该种水果的数量。你需要在篮子中至少放入三种不同类型的水果（例如苹果、香蕉、芒果），并且所有水果的总数量至少为五个。

### 相关知识点：

- **HashMap的使用**：`HashMap`是Rust标准库中提供的一个基于哈希表的键值对集合。它允许你存储唯一的键与值的映射，可用于快速查找。

- **插入项到HashMap**：通过`insert`方法可以向`HashMap`中添加新的键值对。如果插入的键已经存在，则新值会替换旧值。

- **初始化HashMap**：使用`HashMap::new()`方法可以创建一个新的空`HashMap`。

### 解题方法：

#### 创建并填充HashMap
- **步骤描述**：
  1. 使用`HashMap::new()`初始化一个新的`HashMap`变量`basket`。
  2. 使用`insert`方法向`basket`中添加至少三种不同类型的水果及其数量。确保总数量至少为五个。
  3. 返回填充好的`basket`。

#### 代码示例：

```rust
use std::collections::HashMap;

fn fruit_basket() -> HashMap<String, u32> {
    let mut basket = HashMap::new(); // 初始化一个新的HashMap

    // 已经给出的两个香蕉
    basket.insert(String::from("banana"), 2);

    // 添加更多的水果到篮子中
    basket.insert(String::from("apple"), 1);
    basket.insert(String::from("mango"), 2);

    basket
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn at_least_three_types_of_fruits() {
        let basket = fruit_basket();
        assert!(basket.len() >= 3);
    }

    #[test]
    fn at_least_five_fruits() {
        let basket = fruit_basket();
        assert!(basket.values().sum::<u32>() >= 5);
    }
}
```

通过完成这个练习，你将掌握`HashMap`的基本使用方法，包括如何创建`HashMap`、向其中添加元素、以及如何根据需求计算集合内元素的总和。掌握这些技能对于处理和组织大量数据非常有用，特别是在需要快速访问和更新键值对数据时。

## 扩展知识点与解答：

在完成`hashmaps1`练习之后，你已经掌握了如何使用`HashMap`来存储和管理键值对数据。这里的关键点是学会如何有效地使用`HashMap`来处理复杂的数据集合。以下是与本题相关的一些扩展知识点和解题方法，旨在帮助你深入理解`HashMap`以及如何在更广泛的场景下使用它。

### 扩展知识点：

1. **哈希函数**：
   - `HashMap`使用哈希函数来决定如何在内存中存储键值对。了解哈希函数的工作原理可以帮助你更好地理解`HashMap`的性能特性。

2. **处理冲突**：
   - 当两个键产生相同的哈希值时，会发生冲突。`HashMap`有机制来处理这种冲突，但了解这些机制可以帮助你设计更高效的键类型。

3. **所有权**：
   - 向`HashMap`插入值时，键和值的所有权会被转移给`HashMap`。理解所有权的规则对于管理`HashMap`中数据的生命周期至关重要。

4. **可变引用**：
   - 当你需要修改`HashMap`中的值时，需要获取值的可变引用。使用`get_mut`方法可以安全地做到这一点。

### 扩展解题方法：

1. **遍历HashMap**：
   - 使用`iter`或`iter_mut`方法可以遍历`HashMap`的键值对。这对于批量处理或查询`HashMap`中的数据非常有用。

2. **更新值**：
   - `HashMap`提供了`entry` API，允许你根据键的存在与否来方便地更新值。这是处理条件插入和更新操作的推荐方式。

3. **默认值**：
   - 结合`or_insert`方法，你可以为`HashMap`中缺失的键提供默认值。这在累加值或构建计数器时特别有用。

#### 示例代码：

```rust
use std::collections::HashMap;

fn update_fruit_counts() {
    let mut basket = HashMap::new();
    // 假设我们需要更新某个水果的数量
    let fruit = String::from("apple");
    let count = basket.entry(fruit).or_insert(0);
    *count += 1;
}

fn main() {
    let mut basket = HashMap::new();
    // 初始化篮子并添加一些水果
    basket.insert(String::from("banana"), 2);
    basket.insert(String::from("apple"), 1);

    // 遍历篮子并打印每种水果的数量
    for (fruit, count) in &basket {
        println!("{}: {}", fruit, count);
    }

    // 更新水果数量
    update_fruit_counts();
}
```

通过这些扩展内容，你将能够更全面地理解和运用`HashMap`来处理复杂的数据集合，提高你的Rust编程能力。`HashMap`是Rust标准库中非常强大和灵活的工具，掌握它的使用对于构建高效和健壮的Rust应用至关重要。
