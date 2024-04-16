# Exercise 76

- Name: ```iterators5```
- Path: ```exercises/iterators/iterators5.rs```
#### Hint: 

The documentation for the std::iter::Iterator trait contains numerous methods that would be helpful here.

The collection variable in count_collection_iterator is a slice of HashMaps. It needs to be converted into an iterator in order to use the iterator methods.

The fold method can be useful in the count_collection_iterator function.

For a further challenge, consult the documentation for Iterator to find a different method that could make your code more compact than using fold.


---



```rust,editable
// iterators5.rs
//
// Let's define a simple model to track Rustlings exercise progress. Progress
// will be modelled using a hash map. The name of the exercise is the key and
// the progress is the value. Two counting functions were created to count the
// number of exercises with a given progress. Recreate this counting
// functionality using iterators. Try not to use imperative loops (for, while).
// Only the two iterator methods (count_iterator and count_collection_iterator)
// need to be modified.
//
// Execute `rustlings hint iterators5` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

use std::collections::HashMap;

#[derive(Clone, Copy, PartialEq, Eq)]
enum Progress {
    None,
    Some,
    Complete,
}

fn count_for(map: &HashMap<String, Progress>, value: Progress) -> usize {
    let mut count = 0;
    for val in map.values() {
        if val == &value {
            count += 1;
        }
    }
    count
}

fn count_iterator(map: &HashMap<String, Progress>, value: Progress) -> usize {
    // map is a hashmap with String keys and Progress values.
    // map = { "variables1": Complete, "from_str": None, ... }
    todo!();
}

fn count_collection_for(collection: &[HashMap<String, Progress>], value: Progress) -> usize {
    let mut count = 0;
    for map in collection {
        for val in map.values() {
            if val == &value {
                count += 1;
            }
        }
    }
    count
}

fn count_collection_iterator(collection: &[HashMap<String, Progress>], value: Progress) -> usize {
    // collection is a slice of hashmaps.
    // collection = [{ "variables1": Complete, "from_str": None, ... },
    //     { "variables2": Complete, ... }, ... ]
    todo!();
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn count_complete() {
        let map = get_map();
        assert_eq!(3, count_iterator(&map, Progress::Complete));
    }

    #[test]
    fn count_some() {
        let map = get_map();
        assert_eq!(1, count_iterator(&map, Progress::Some));
    }

    #[test]
    fn count_none() {
        let map = get_map();
        assert_eq!(2, count_iterator(&map, Progress::None));
    }

    #[test]
    fn count_complete_equals_for() {
        let map = get_map();
        let progress_states = vec![Progress::Complete, Progress::Some, Progress::None];
        for progress_state in progress_states {
            assert_eq!(
                count_for(&map, progress_state),
                count_iterator(&map, progress_state)
            );
        }
    }

    #[test]
    fn count_collection_complete() {
        let collection = get_vec_map();
        assert_eq!(
            6,
            count_collection_iterator(&collection, Progress::Complete)
        );
    }

    #[test]
    fn count_collection_some() {
        let collection = get_vec_map();
        assert_eq!(1, count_collection_iterator(&collection, Progress::Some));
    }

    #[test]
    fn count_collection_none() {
        let collection = get_vec_map();
        assert_eq!(4, count_collection_iterator(&collection, Progress::None));
    }

    #[test]
    fn count_collection_equals_for() {
        let progress_states = vec![Progress::Complete, Progress::Some, Progress::None];
        let collection = get_vec_map();

        for progress_state in progress_states {
            assert_eq!(
                count_collection_for(&collection, progress_state),
                count_collection_iterator(&collection, progress_state)
            );
        }
    }

    fn get_map() -> HashMap<String, Progress> {
        use Progress::*;

        let mut map = HashMap::new();
        map.insert(String::from("variables1"), Complete);
        map.insert(String::from("functions1"), Complete);
        map.insert(String::from("hashmap1"), Complete);
        map.insert(String::from("arc1"), Some);
        map.insert(String::from("as_ref_mut"), None);
        map.insert(String::from("from_str"), None);

        map
    }

    fn get_vec_map() -> Vec<HashMap<String, Progress>> {
        use Progress::*;

        let map = get_map();

        let mut other = HashMap::new();
        other.insert(String::from("variables2"), Complete);
        other.insert(String::from("functions2"), Complete);
        other.insert(String::from("if1"), Complete);
        other.insert(String::from("from_into"), None);
        other.insert(String::from("try_from_into"), None);

        vec![map, other]
    }
}
```

---

### 本题内容

本练习的目标是通过使用 Rust 的迭代器方法来计算特定状态的进度数量，这是从基于循环的命令式编程转向更声明式和函数式的编程风格。通过替代传统的 `for` 循环，学生将学习如何使用 Rust 的迭代器方法更有效、更简洁地处理集合数据。

### 相关知识点

1. **迭代器的使用**：
   - 迭代器提供了一种遍历集合的方法，支持多种操作，如转换、过滤、折叠等，这些操作可以链式调用，简化代码。

2. **`count` 方法**：
   - 这是一个迭代器方法，用于计算迭代器中满足某种条件的元素数量。

3. **`fold` 方法**：
   - `fold` 允许你通过一个累积函数和一个初始累积值来将所有元素折叠（或合并）成一个单一值。这在进行累计计算时非常有用。

4. **`flat_map` 方法**：
   - 适用于处理嵌套结构（如集合的集合），可以将嵌套的迭代器“展平”成一个单一的迭代器，这对于本题中的集合数组尤其有用。

### 解题方法

#### 步骤描述：

**Step 1: 修改 `count_iterator` 方法**
- 使用 `map` 方法来遍历 HashMap 的值。
- 使用 `filter` 方法筛选出等于给定 `Progress` 值的元素。
- 使用 `count` 方法计数并返回结果。

**Step 2: 修改 `count_collection_iterator` 方法**
- 使用 `flat_map` 将数组中的多个 HashMap 展平成单个迭代器。
- 同样使用 `filter` 和 `count` 方法来统计满足条件的进度值。

#### 代码示例：

```rust
fn count_iterator(map: &HashMap<String, Progress>, value: Progress) -> usize {
    map.values()
        .filter(|&progress| *progress == value)
        .count()
}

fn count_collection_iterator(collection: &[HashMap<String, Progress>], value: Progress) -> usize {
    collection.iter()
        .flat_map(|map| map.values())
        .filter(|&progress| *progress == value)
        .count()
}
```

#### 详细说明：

- **count_iterator**:
  - `map.values()` 创建一个只包含值的迭代器。
  - `filter(|&progress| *progress == value)` 筛选出与指定 `Progress` 状态相匹配的元素。
  - `count()` 计算筛选后的元素数量。

- **count_collection_iterator**:
  - `collection.iter()` 遍历包含多个 HashMap 的切片。
  - `flat_map(|map| map.values())` 展平每个 HashMap 中的值，创建一个包含所有值的迭代器。
  - 使用 `filter` 和 `count` 进行筛选和计数，同上。

通过这些步骤和代码示例，可以更好地理解如何在 Rust 中利用迭代器来进行更高效和简洁的数据处理。这种方法不仅减少了代码量，还提高了代码的可读性和可维护性。

## 扩展知识点与解答：

### 扩展知识点

1. **更多迭代器方法**：
   - **`any` 和 `all`**：这两个方法用于检查迭代器中的元素是否符合某个条件。`any` 返回 `true` 如果任意元素满足条件，而 `all` 返回 `true` 如果所有元素满足条件。
   - **`find` 和 `filter_map`**：`find` 方法用于查找第一个符合条件的元素并返回一个 `Option` 类型。`filter_map` 允许同时过滤和映射集合，提供了一个强大的方式来处理数据转换和筛选。

2. **并行迭代器**：
   - 使用 `rayon` 这样的库可以实现迭代器的并行处理。并行迭代器对于处理大规模数据集或在性能至关重要的情况下非常有用。

3. **惰性求值与性能**：
   - Rust 的迭代器是惰性的，这意味着计算只会在实际需要结果时进行。这种方法可以优化性能，尤其是在链式调用多个操作时，可以避免创建不必要的中间集合。

### 扩展解题方法

1. **结合使用 `filter_map`**：
   - 你可以使用 `filter_map` 来简化对集合的处理。例如，在统计具有特定 `Progress` 值的条目时，`filter_map` 可以在单个步骤中完成过滤和映射操作，减少代码的复杂性。

2. **错误处理和有效性检查**：
   - 在处理复杂数据时，添加适当的错误处理和有效性检查是很重要的。例如，确保在处理之前集合或其元素不为空，可以避免运行时错误。

3. **优化性能**：
   - 考虑使用并行迭代器来优化对大型数据集的处理。此外，评估不同迭代器方法对性能的影响，选择最适合当前数据和处理需求的方法。

#### 扩展代码示例：使用 `filter_map`

假设你想统计只有完成状态（`Complete`）的项，并且还想从键中提取某些信息（比如获取前缀），可以使用 `filter_map`：

```rust
fn count_complete_with_prefix(collection: &[HashMap<String, Progress>], prefix: &str) -> usize {
    collection.iter()
        .flat_map(|map| map.iter())
        .filter_map(|(key, &value)|
            if value == Progress::Complete && key.starts_with(prefix) {
                Some(())
            } else {
                None
            })
        .count()
}
```

通过这种方式，你可以在单个操作中完成过滤和映射，使代码更简洁并可能更高效。这种方法特别适合于需要对数据进行多重条件筛选和转换的场景。

### 测试用例解释

1. **测试单个 HashMap 的计数**：
   - `count_complete`、`count_some`、`count_none`：这些测试用例分别检查在单个 `HashMap` 中，状态为 `Complete`、`Some` 和 `None` 的条目数量是否正确。它们使用 `count_iterator` 函数来计算并比较预期的结果。
   - 这些测试确保了 `count_iterator` 函数能够准确地对不同进度状态的条目进行计数。

2. **测试多个 HashMap 的计数**：
   - `count_collection_complete`、`count_collection_some`、`count_collection_none`：这些测试用例检查在由多个 `HashMap` 组成的数组中，不同进度状态的条目数量是否被正确计算。它们使用 `count_collection_iterator` 函数。
   - 这些测试验证了 `count_collection_iterator` 函数是否能正确处理包含多个 `HashMap` 的集合，且正确累加各个 `HashMap` 中相应状态的条目数。

3. **对比命令式和迭代器方法的结果**：
   - `count_complete_equals_for` 和 `count_collection_equals_for`：这些测试用例对比命令式循环实现（`count_for` 和 `count_collection_for`）与迭代器方法实现（`count_iterator` 和 `count_collection_iterator`）的结果是否一致。
   - 这些测试保证迭代器方法不仅实现了相同的功能，而且与传统命令式方法的结果完全相同，确保迭代器实现的正确性和有效性。
4. **测试数据的设置**
   - 通过 `get_map` 和 `get_vec_map` 函数，测试构建了包含特定 `Progress` 状态的 `HashMap` 和 `HashMap` 的数组。这些函数提供了用于测试的固定数据集，确保测试结果的可重复性和准确性。
