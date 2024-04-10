# Exercise 45

- Name: ```hashmaps2```
- Path: ```exercises/hashmaps/hashmaps2.rs```
#### Hint: 

Use the `entry()` and `or_insert()` methods of `HashMap` to achieve this.
Learn more at [https://doc.rust-lang.org/stable/book/ch08-03-hash-maps.html]( https://doc.rust-lang.org/stable/book/ch08-03-hash-maps.html#only-inserting-a-value-if-the-key-has-no-value) ([Chinese version](https://rustwiki.org/zh-CN/book/ch08-03-hash-maps.html#%E5%8F%AA%E5%9C%A8%E9%94%AE%E6%B2%A1%E6%9C%89%E5%AF%B9%E5%BA%94%E5%80%BC%E6%97%B6%E6%8F%92%E5%85%A5))



---



```rust,editable
// hashmaps2.rs
//
// We're collecting different fruits to bake a delicious fruit cake. For this,
// we have a basket, which we'll represent in the form of a hash map. The key
// represents the name of each fruit we collect and the value represents how
// many of that particular fruit we have collected. Three types of fruits -
// Apple (4), Mango (2) and Lychee (5) are already in the basket hash map. You
// must add fruit to the basket so that there is at least one of each kind and
// more than 11 in total - we have a lot of mouths to feed. You are not allowed
// to insert any more of these fruits!
//
// Make me pass the tests!
//
// Execute `rustlings hint hashmaps2` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

use std::collections::HashMap;

#[derive(Hash, PartialEq, Eq)]
enum Fruit {
    Apple,
    Banana,
    Mango,
    Lychee,
    Pineapple,
}

fn fruit_basket(basket: &mut HashMap<Fruit, u32>) {
    let fruit_kinds = vec![
        Fruit::Apple,
        Fruit::Banana,
        Fruit::Mango,
        Fruit::Lychee,
        Fruit::Pineapple,
    ];

    for fruit in fruit_kinds {
        // TODO: Insert new fruits if they are not already present in the
        // basket. Note that you are not allowed to put any type of fruit that's
        // already present!
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    // Don't modify this function!
    fn get_fruit_basket() -> HashMap<Fruit, u32> {
        let mut basket = HashMap::<Fruit, u32>::new();
        basket.insert(Fruit::Apple, 4);
        basket.insert(Fruit::Mango, 2);
        basket.insert(Fruit::Lychee, 5);

        basket
    }

    #[test]
    fn test_given_fruits_are_not_modified() {
        let mut basket = get_fruit_basket();
        fruit_basket(&mut basket);
        assert_eq!(*basket.get(&Fruit::Apple).unwrap(), 4);
        assert_eq!(*basket.get(&Fruit::Mango).unwrap(), 2);
        assert_eq!(*basket.get(&Fruit::Lychee).unwrap(), 5);
    }

    #[test]
    fn at_least_five_types_of_fruits() {
        let mut basket = get_fruit_basket();
        fruit_basket(&mut basket);
        let count_fruit_kinds = basket.len();
        assert!(count_fruit_kinds >= 5);
    }

    #[test]
    fn greater_than_eleven_fruits() {
        let mut basket = get_fruit_basket();
        fruit_basket(&mut basket);
        let count = basket.values().sum::<u32>();
        assert!(count > 11);
    }
    
    #[test]
    fn all_fruit_types_in_basket() {
        let mut basket = get_fruit_basket();
        fruit_basket(&mut basket);
        for amount in basket.values() {
            assert_ne!(amount, &0);
        }
    }
}

```

---

### 本题内容：

本练习的目标是向已有的水果篮（`HashMap`）中添加不同种类的水果，确保篮子中至少有五种不同类型的水果，且总数超过11个。需要注意的是，篮子中已经有了苹果（4个）、芒果（2个）和荔枝（5个），你不能再添加这些水果。

### 相关知识点：

- **使用`entry()`和`or_insert()`**：
   - `HashMap`的`entry()`方法返回一个枚举`Entry`，它代表一个可能存在也可能不存在的值。`or_insert()`方法则可以在键不存在时插入一个默认值。这两个方法结合起来非常适合确保`HashMap`中的键有值，而不覆盖已存在的值。

- **枚举与HashMap**：
   - 在本练习中，使用枚举`Fruit`作为`HashMap`的键。这是在Rust中处理固定集合键时的一种常见模式。

### 解题方法：

#### 检查并插入新水果
- **步骤描述**：
  1. 遍历`fruit_kinds`向量中定义的所有水果类型。
  2. 对于每种水果，使用`entry()`方法检查它是否已经存在于篮子中。
  3. 对于不存在于篮子中的水果，使用`or_insert()`方法插入默认数量1。

#### 代码示例：

```rust
use std::collections::HashMap;

#[derive(Hash, PartialEq, Eq)]
enum Fruit {
    Apple,
    Banana,
    Mango,
    Lychee,
    Pineapple,
}

fn fruit_basket(basket: &mut HashMap<Fruit, u32>) {
    let fruit_kinds = vec![
        Fruit::Apple,
        Fruit::Banana,
        Fruit::Mango,
        Fruit::Lychee,
        Fruit::Pineapple,
    ];

    for fruit in fruit_kinds {
        basket.entry(fruit).or_insert(1);
    }
    // 确保总数超过11，可以对一种水果添加额外数量
    *basket.entry(Fruit::Pineapple).or_insert(1) += (12 - basket.values().sum::<u32>());
}

#[cfg(test)]
mod tests {
    use super::*;

    // Don't modify this function!
    fn get_fruit_basket() -> HashMap<Fruit, u32> {
        let mut basket = HashMap::<Fruit, u32>::new();
        basket.insert(Fruit::Apple, 4);
        basket.insert(Fruit::Mango, 2);
        basket.insert(Fruit::Lychee, 5);

        basket
    }

    // 测试省略...
}
```

通过实施这个解决方案，你不仅能满足题目要求，还能灵活运用`HashMap`的`entry()`和`or_insert()`方法来处理数据。这种模式在需要对集合中的元素进行条件插入时非常有用，是Rust编程中常见的一个模式。掌握这些方法将大大提升你在处理集合类型数据时的效率和灵活性。

## 扩展知识点与解答：

完成`hashmaps2`练习后，你已经了解了如何利用`entry()`和`or_insert()`方法来高效地处理`HashMap`中的数据。这些技巧对于避免不必要的查找操作、优化性能以及处理复杂的数据更新逻辑至关重要。下面，我们将探讨与这个题目相关的一些扩展知识点和解题思路，帮助你深入理解并灵活运用`HashMap`。

### 扩展知识点：

1. **更复杂的更新逻辑**：
   - 使用`entry().or_insert()`模式不仅可以用于插入默认值，还可以结合复杂的更新逻辑，比如基于旧值计算新值。

2. **`or_insert_with()`方法**：
   - 当默认值的获取逻辑比较昂贵或复杂时，可以使用`or_insert_with()`来延迟计算默认值。这个方法接受一个闭包，只有在键确实不存在时，才会调用这个闭包。

3. **避免哈希冲突**：
   - 在选择`HashMap`键时考虑其哈希性能。避免低质量的哈希函数可能导致的哈希冲突，这对提高`HashMap`的性能很重要。

### 扩展解题方法：

#### 统计单词频率
- **步骤描述**：
  1. 遍历文本中的每个单词。
  2. 使用`entry().or_insert(0)`检查每个单词是否已在`HashMap`中，若不存在则插入值0。
  3. 对于每个单词，通过`*entry += 1;`更新其出现次数。

#### 代码示例：

```rust
use std::collections::HashMap;

fn count_word_frequencies(text: &str) -> HashMap<String, u32> {
    let mut frequencies = HashMap::new();
    for word in text.split_whitespace() {
        let count = frequencies.entry(word.to_string()).or_insert(0);
        *count += 1;
    }
    frequencies
}

fn main() {
    let text = "hello world hello rust";
    let frequencies = count_word_frequencies(text);
    println!("{:?}", frequencies);
    // 输出：{"hello": 2, "world": 1, "rust": 1}
}
```

通过掌握`HashMap`的`entry()`和`or_insert()`方法，你可以有效地管理和更新键值对数据，处理像单词频率统计等复杂的场景。这些方法不仅提高了代码的效率，还使得代码更加清晰和易于维护。深入理解并灵活运用这些方法，将使你能够更加自如地处理Rust中的集合数据。
