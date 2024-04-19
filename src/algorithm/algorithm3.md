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

---

## 其他排序方式

在 Rust 中实现排序有多种方法，根据不同的应用场景和性能需求，可以选择不同的排序算法。以下是一些常见的排序方法和如何在 Rust 中实现它们：

1. **内置排序方法**：
   Rust 的标准库提供了稳定的排序算法，主要是通过 `sort()` 和 `sort_unstable()` 方法：
   - `sort()`: 这是一个稳定的排序方法，通常使用归并排序实现，适用于需要保持相等元素之间顺序的场景。
   - `sort_unstable()`: 这是一个非稳定的排序方法，通常性能略优于 `sort()`，适用于不需要维护元素间原始顺序的场景。

   ```rust
   let mut vec = vec![3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5];
   vec.sort();  // 使用稳定排序
   vec.sort_unstable();  // 使用非稳定排序
   ```

2. **手动实现常见排序算法**：
   
   1. **冒泡排序**：通过重复遍历要排序的列表，比较相邻元素并交换顺序错误的元素，逐步将最大或最小的元素“冒泡”到列表的一端。
   2. **选择排序**：从待排序的数据中找到最小（或最大）的元素，存放到排序序列的起始位置，然后，再从剩余未排序元素中继续寻找最小（或最大）元素，然后放到已排序序列的末尾。
   3. **插入排序**：构建有序序列，对于未排序数据，在已排序序列中从后向前扫描，找到相应位置并插入。
   4. **希尔排序**：一种基于插入排序的快速排序算法，通过比较相距一定间隔的元素来工作，间隔逐渐减少到1，使得数据变得越来越接近于有序，最后进行一次直接插入排序。
   5. **归并排序**：采用分治法的一个非常典型的应用。将已有序的子序列合并，得到完全有序的序列；即先使每个子序列有序，再使子序列段间有序。
   6. **快速排序**：通过一次排序将要排序的数据分割成独立的两部分，其中一部分的所有数据都比另外一部分的所有数据要小，然后再按此方法对这两部分数据分别进行快速排序，整个排序过程可以递归进行。
   7. **堆排序**：是指利用堆这种数据结构所设计的一种排序算法。堆积是一个近似完全二叉树的结构，并同时满足堆积的性质：即子节点的键值或索引总是小于（或者大于）它的父节点。
   8. **计数排序**：是一个非基于比较的排序算法，其核心在于将输入的数据值转化为键存储在额外开辟的数组空间中。作为一种线性时间复杂度的排序，计数排序要求输入的数据必须是有确定范围的整数。
   9. **桶排序**：假设输入数据服从均匀分布，将数据分到有限数量的桶里，每个桶再分别排序（有可能再使用别的排序算法或是以递归方式继续使用桶排序进行排序）。
   10. **基数排序**：是按照低位先排序，然后收集；再按照高位排序，然后再收集；以此类推，直到最高位。有时候有些属性是有优先顺序的，先按低优先度排序，再按高优先度排序。
   
3. **使用第三方库**：
   在 Rust 生态中，也有一些第三方库提供了更多的排序算法实现，例如使用 `ndarray` 和 `rayon` 等库进行并行排序，这可以在处理大规模数据时显著提高效率。

每种排序方法都有其适用场景和性能特点，选择合适的排序算法可以根据具体需求和数据特性来决定。在实际应用中，通常内置的排序方法已足够应对大多数情况。对于特殊需求，例如需要特别优化的性能或处理非常大的数据集，可能需要考虑手动实现或使用专门的库来达到最优效果。

### 1. 冒泡排序

冒泡排序是一种简单的排序算法，它重复地遍历列表，比较相邻元素并在顺序错误时交换它们。这个过程重复进行，直到没有必要的交换为止，这意味着列表已经排序完成。

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
        n -= 1;  // 减小n的值，因为每次循环的最大元素都会被放到正确的位置
    }
}
```

**解释代码**：

- **函数定义**：
  - `bubble_sort` 函数接受一个可变引用到一个切片，该切片中的元素类型必须实现了 `Ord` trait，以保证元素间可以进行比较。

- **循环逻辑**：
  - 使用一个布尔变量 `swapped` 来跟踪是否发生了交换。如果一次遍历中没有进行任何交换，说明数组已经排序完成，可以提前退出循环。
  - 外层循环继续直到一次完整的遍历没有发生任何交换。
  - 内层循环遍历数组元素，比较并交换顺序错误的相邻元素。

- **性能注意事项**：
  - 冒泡排序是一种效率较低的排序算法，尤其是在数据量大时。它的平均时间复杂度和最坏时间复杂度都是 \(O(n^2)\)，其中 \(n\) 是数组长度。

这种排序方法虽简单，但不推荐用于大量数据的排序。在实际应用中，通常会选择更高效的排序算法，如快速排序、归并排序等。

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

### 4.希尔排序

希尔排序是一种基于插入排序的算法，通过先比较距离较远的元素来重排数据，使得数据可以更快地接近其排序位置。这个排序算法是由Donald Shell于1959年提出的，它是第一个突破O(n^2)时间复杂度的排序算法，虽然它最坏的时间复杂度依然是O(n^2)。

希尔排序的基本思想是将数组中的所有元素分成多个子序列，然后分别进行插入排序，所不同的是，这些子序列是由相隔一定间隔的元素组成的。随着算法的进行，间隔会逐渐减小，直到减小到1，这时整个数组就是一个子序列，算法进行最后一次插入排序后数组就完全有序了。

下面是希尔排序的一个Rust实现，使用了一个简单的间隔序列（通常称为增量序列），从 n/2 开始，每次除以 2：

```rust
fn shell_sort<T: Ord>(arr: &mut [T]) {
    let mut gap = arr.len() / 2; // 初始化间隔
    while gap > 0 {
        for i in gap..arr.len() {
            let mut j = i;
            let temp = arr[i].clone(); // 取出无序列表中的元素
            // 进行插入排序
            while j >= gap && arr[j - gap] > temp {
                arr[j] = arr[j - gap].clone();
                j -= gap;
            }
            arr[j] = temp;
        }
        gap /= 2; // 减少间隔
    }
}

fn main() {
    let mut vec = vec![22, 7, 1, 99, 33, 15, 62, 84, 9];
    println!("Original vector: {:?}", vec);
    shell_sort(&mut vec);
    println!("Sorted vector: {:?}", vec);
}
```

**代码解释：**

- **间隔的选择**：希尔排序的性能在很大程度上依赖于间隔的选择。在这个实现中，我们从 `arr.len() / 2` 开始，每次将间隔除以 2，直到间隔为 1。

- **外层循环**：控制间隔的大小，每一次循环后都将间隔减半。

- **内层循环**：从当前间隔开始遍历数组。每个元素都会与它之前间隔为 `gap` 的元素进行比较，并根据比较结果可能进行位置交换，这是插入排序的核心步骤。

- **插入排序过程**：如果在位置 `j` 的元素小于位置 `j-gap` 的元素，则将位置 `j-gap` 的元素向后移动一个间隔，并逐步降低 `j` 的值（即向前移动）。当找到合适的位置后，将取出的元素放入该位置。

这种方式的好处是开始时 `gap` 较大，可以快速减少大范围的无序状态，然后随着 `gap` 的减小，虽然每个子序列的元素较多，但它们已经接近于排序状态，这样可以减少比较和移动的次数。

希尔排序是插入排序的改进版，适用于中等大小的数组，其复杂度依赖于所选择的间隔序列。在最优情况下，使用特定的间隔序列可以达到 O(n log n) 的时间复杂度，但这需要精心选择间隔序列。在实际应用中，希尔排序比简单的插入排序要快，尤其是在大规模数组中。

### 5.归并排序

归并排序是一种分治法（Divide and Conquer）的应用，它将一个数组分成两半，分别对它们进行排序，然后将两个有序的部分合并在一起。这种排序方式在最坏、最好和平均情况下都保持 \(O(n \log n)\) 的时间复杂度，而且是一种稳定的排序方法。

```rust
// 主函数，调用这个函数以排序任何实现了 PartialOrd 的元素的可变切片
fn merge_sort<T: PartialOrd + Clone>(arr: &mut [T]) {
    let len = arr.len();
    if len <= 1 {
        return; // 数组长度为 1 或 0 时，无需排序
    }

    let mid = len / 2;
    merge_sort(&mut arr[0..mid]); // 递归排序左半部分
    merge_sort(&mut arr[mid..]); // 递归排序右半部分

    // 合并已排序的两半
    let mut merge = vec![];
    merge_combine(&arr[0..mid], &arr[mid..], &mut merge);
    arr.copy_from_slice(&merge); // 将排序后的数据复制回原切片
}

// 辅助函数，用于合并两个已排序的切片
fn merge_combine<T: PartialOrd + Clone>(left: &[T], right: &[T], merge: &mut Vec<T>) {
    let mut left_idx = 0;
    let mut right_idx = 0;

    // 遍历两个切片，将较小的元素依次添加到 merge 中
    while left_idx < left.len() && right_idx < right.len() {
        if left[left_idx] <= right[right_idx] {
            merge.push(left[left_idx].clone());
            left_idx += 1;
        } else {
            merge.push(right[right_idx].clone());
            right_idx += 1;
        }
    }

    // 如果左边切片仍有剩余元素，将它们添加到 merge 中
    while left_idx < left.len() {
        merge.push(left[left_idx].clone());
        left_idx += 1;
    }

    // 如果右边切片仍有剩余元素，将它们添加到 merge 中
    while right_idx < right.len() {
        merge.push(right[right_idx].clone());
        right_idx += 1;
    }
}

fn main() {
    let mut vec = vec![34, 7, 23, 32, 5, 62];
    println!("Original vector: {:?}", vec);
    merge_sort(&mut vec);
    println!("Sorted vector: {:?}", vec);
}
```

**解释代码**：

1. **`merge_sort` 函数**：
    - 这是归并排序的主函数。首先检查数组的长度，如果长度小于等于1，直接返回，因为长度为1或空数组自然是有序的。
    - 然后找到数组的中点，将数组分成两个部分，并对这两部分分别递归调用 `merge_sort`。
    - 在两个部分都排序好之后，调用 `merge_combine` 来将它们合并成一个有序数组。

2. **`merge_combine` 函数**：
    - 这个函数负责合并两个已经排序的数组切片。它使用两个索引 `left_idx` 和 `right_idx` 来追踪两个切片的元素。
    - 比较两个索引指向的元素，将较小的元素放入结果向量 `merge` 中，并移动相应的索引。
    - 最后，如果其中一个切片还有剩余元素，将这些元素追加到 `merge` 中。

3. **主函数**：
    - 在 `main` 函数中，创建一个待排序的向量，调用 `merge_sort` 进行排序，然后打印排序前后的向量。

这种实现方式虽然清晰，但可能在大数据量时内存使用较多，因为它需要频繁地分配和释放内存。在实际应用中，可以优化内存使用，例如通过在排序前分配一个和原数组同样大小的辅助数组，然后在所有的合并操作中重复使用这个数组。

### 6. 堆排序

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
