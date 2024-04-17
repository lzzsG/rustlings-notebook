# Exercise 103

- Name: ```algorithm3```
- Path: ```exercises/algorithm/algorithm3.rs```
#### Hint: 

No hints this time!


---



```rust,editable
/*
	sort
	This problem requires you to implement a sorting algorithm
	you can use bubble sorting, insertion sorting, heap sorting, etc.
*/
// I AM NOT DONE

fn sort<T>(array: &mut [T]){
	//TODO
}
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_sort_1() {
        let mut vec = vec![37, 73, 57, 75, 91, 19, 46, 64];
        sort(&mut vec);
        assert_eq!(vec, vec![19, 37, 46, 57, 64, 73, 75, 91]);
    }
	#[test]
    fn test_sort_2() {
        let mut vec = vec![1];
        sort(&mut vec);
        assert_eq!(vec, vec![1]);
    }
	#[test]
    fn test_sort_3() {
        let mut vec = vec![99, 88, 77, 66, 55, 44, 33, 22, 11];
        sort(&mut vec);
        assert_eq!(vec, vec![11, 22, 33, 44, 55, 66, 77, 88, 99]);
    }
}
```

---

### 本题内容：

本练习要求学生实现一个排序算法。排序算法是计算机科学中的基础，重要性不言而喻。在这个练习中，学生可以选择实现冒泡排序、插入排序、选择排序、堆排序等任一算法。目的是让学生理解不同排序算法的工作原理及其效率，并掌握在 Rust 中操作数组和向量的方法。

### 相关知识点：

1. **排序算法的分类**：
   - **冒泡排序**：通过重复遍历要排序的列表，比较每对相邻项，并在顺序错误的情况下交换它们。效率较低，但实现简单。
   - **插入排序**：构建最终排序的数组或列表，每次迭代一个元素通过插入已排序的数组部分在适当位置。
   - **选择排序**：不断选择剩余元素中的最小者，放到排序数组的末尾。
   - **堆排序**：利用堆这种数据结构设计的排序算法，具有较好的时间复杂度。

2. **泛型**：
   - 泛型允许代码对多种数据类型进行操作，增加了代码的灵活性和复用性。
   - 在 Rust 中使用 `<T>` 来标示泛型类型，`T` 可以被任何类型替换。

3. **Trait Bounds**：
   - 对于排序操作，元素类型需要能够进行比较和排序，通常需要类型 `T` 实现了 `PartialOrd` 和 `Ord` trait。
   - 使用 `T: Ord` 约束可以确保元素可排序。

### 解题方法：

1. **选择排序算法**：根据自己的熟悉程度选择一种排序算法来实现。
2. **实现排序函数**：
   - 使用泛型函数 `fn sort<T: Ord>(array: &mut [T])` 来确保函数可以对任何可排序的元素类型进行操作。
   - 实现具体的排序逻辑，确保能够将输入数组按升序排序。
3. **测试代码的正确性**：
   - 使用提供的单元测试来验证排序函数的正确性。
   - 可以添加额外的测试用例以覆盖边缘情况，例如空数组或数组中包含重复元素的情况。

### 代码示例：插入排序

```rust
fn sort<T: Ord>(array: &mut [T]) {
    for i in 1..array.len() {
        let mut j = i;
        while j > 0 && array[j] < array[j - 1] {
            array.swap(j, j - 1);
            j -= 1;
        }
    }
}
```

此代码使用了泛型 `T`，其中 `T` 必须实现 `Ord` trait，以确保元素可以进行比较和排序。这个示例中的 `sort` 函数实现了插入排序算法，适用于一般的排序需求。

## 其他排序方式

### 1. 冒泡排序

冒泡排序是最简单的排序算法之一，它重复地遍历列表，比较相邻的元素，并在顺序错误时交换它们。

```rust
fn bubble_sort<T: Ord>(array: &mut [T]) {
    let mut n = array.len();
    let mut swapped = true;
    while swapped {
        swapped = false;
        for i in 1..n {
            if array[i - 1] > array[i] {
                array.swap(i - 1, i);
                swapped = true;
            }
        }
        n -= 1;  // 减少每次遍历的次数
    }
}
```

### 2. 选择排序

选择排序通过重复地找出未排序部分的最小元素，并将其放在已排序部分的末尾来工作。

```rust
fn selection_sort<T: Ord>(array: &mut [T]) {
    for i in 0..array.len() {
        let mut min_index = i;
        for j in i + 1..array.len() {
            if array[j] < array[min_index] {
                min_index = j;
            }
        }
        array.swap(i, min_index);
    }
}
```

### 3. 快速排序

快速排序是一个高效的排序算法，采用分而治之的方法，它将大问题分解为小问题来解决。

```rust
fn quick_sort<T: Ord>(array: &mut [T]) {
    if array.len() <= 1 {
        return;
    }
    let pivot_index = partition(array);
    quick_sort(&mut array[0..pivot_index]);
    quick_sort(&mut array[pivot_index + 1..]);
}

fn partition<T: Ord>(array: &mut [T]) -> usize {
    let pivot_index = array.len() / 2;
    array.swap(pivot_index, array.len() - 1);
    let mut i = 0;
    for j in 0..array.len() - 1 {
        if array[j] <= array[array.len() - 1] {
            array.swap(i, j);
            i += 1;
        }
    }
    array.swap(i, array.len() - 1);
    i
}
```

### 4. 堆排序

堆排序是基于堆数据结构的一种有效的排序方法。它首先将列表转换为一个最大堆，然后逐步从堆中取出最大的元素并恢复堆的性质。

```rust
fn heap_sort<T: Ord>(array: &mut [T]) {
    let end = array.len();
    // 构建最大堆
    for start in (0..end / 2).rev() {
        sift_down(array, start, end - 1);
    }
    // 堆排序
    for end in (1..array.len()).rev() {
        array.swap(0, end);
        sift_down(array, 0, end - 1);
    }
}

fn sift_down<T: Ord>(array: &mut [T], start: usize, end: usize) {
    let mut root = start;
    while root * 2 + 1 <= end {
        let child = root * 2 + 1;
        let mut swap = root;
        if array[swap] < array[child] {
            swap = child;
        }
        if child + 1 <= end && array[swap] < array[child + 1] {
            swap = child + 1;
        }
        if swap != root {
            array.swap(root, swap);
            root = swap;
        } else {
            break;
        }
    }
}
```

## 扩展知识点与解答：

### 扩展知识点

1. **排序算法的稳定性**：
   - 稳定的排序算法会保持等值元素的原始顺序。这对于保持多字段排序的结果很重要。例如，插入排序和冒泡排序是稳定的，而选择排序和快速排序则不是。

2. **排序算法的复杂度**：
   - 算法的时间复杂度可以决定其在不同数据规模下的表现。例如，冒泡排序和插入排序在最坏情况下具有 \(O(n^2)\) 的时间复杂度，而快速排序通常表现为 \(O(n \log n)\)。

3. **原地排序**：
   - 原地排序算法尽量减少对额外内存的需求，如快速排序和堆排序。这些算法尤其适用于内存使用受限的场景。

4. **非比较排序**：
   - 除了比较元素大小的排序方法外，还有基于数值特性的非比较排序算法，如计数排序、桶排序和基数排序。这些算法在处理特定类型的数据时，如整数或固定长度的字符串时，可以提供超过 \(O(n \log n)\) 的性能。

### 扩展解题方法

1. **选择适当的排序算法**：
   - 根据数据的特性和需求选择合适的排序算法。例如，如果数据基本有序，插入排序将非常高效。

2. **优化排序算法**：
   - 对于基本的排序算法，考虑实现一些优化策略，如对快速排序使用三数取中法来选择枢轴，或者在递归深度过大时切换到堆排序。

3. **并行化**：
   - 考虑使用并行算法来加速排序过程，特别是在大数据集上。例如，可以并行化的归并排序和快速排序。

4. **使用 Rust 的特性**：
   - 利用 Rust 的所有权和借用规则来优化内存使用，减少不必要的数据复制。
   - 利用 Rust 的泛型和特性来写出可以处理任何可排序数据类型的函数。

### 代码示例：使用三数取中法优化快速排序

```rust
fn quick_sort<T: Ord>(array: &mut [T]) {
    if array.len() <= 1 {
        return;
    }
    let pivot_index = median_of_three(&array);
    array.swap(pivot_index, array.len() - 1);
    let pivot_index = partition(array);
    quick_sort(&mut array[0..pivot_index]);
    quick_sort(&mut array[pivot_index + 1..]);
}

fn median_of_three<T: Ord>(array: &[T]) -> usize {
    let last = array.len() - 1;
    let mid = last / 2;
    if array[0] > array[mid] {
        if array[mid] > array[last] { mid }
        else if array[0] > array[last] { last }
        else { 0 }
    } else {
        if array[0] > array[last] { 0 }
        else if array[mid] > array[last] { last }
        else { mid }
    }
}
```
这种优化可以减少快速排序在最坏情况下的性能下降，使其在多种数据分布情况下表现更加稳定。
