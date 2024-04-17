# Exercise 109

- Name: ```algorithm9```
- Path: ```exercises/algorithm/algorithm9.rs```
#### Hint: 

No hints this time!


---



```rust,editable
/*
	heap
	This question requires you to implement a binary heap function
*/
// I AM NOT DONE

use std::cmp::Ord;
use std::default::Default;

pub struct Heap<T>
where
    T: Default,
{
    count: usize,
    items: Vec<T>,
    comparator: fn(&T, &T) -> bool,
}

impl<T> Heap<T>
where
    T: Default,
{
    pub fn new(comparator: fn(&T, &T) -> bool) -> Self {
        Self {
            count: 0,
            items: vec![T::default()],
            comparator,
        }
    }

    pub fn len(&self) -> usize {
        self.count
    }

    pub fn is_empty(&self) -> bool {
        self.len() == 0
    }

    pub fn add(&mut self, value: T) {
        //TODO
    }

    fn parent_idx(&self, idx: usize) -> usize {
        idx / 2
    }

    fn children_present(&self, idx: usize) -> bool {
        self.left_child_idx(idx) <= self.count
    }

    fn left_child_idx(&self, idx: usize) -> usize {
        idx * 2
    }

    fn right_child_idx(&self, idx: usize) -> usize {
        self.left_child_idx(idx) + 1
    }

    fn smallest_child_idx(&self, idx: usize) -> usize {
        //TODO
		0
    }
}

impl<T> Heap<T>
where
    T: Default + Ord,
{
    /// Create a new MinHeap
    pub fn new_min() -> Self {
        Self::new(|a, b| a < b)
    }

    /// Create a new MaxHeap
    pub fn new_max() -> Self {
        Self::new(|a, b| a > b)
    }
}

impl<T> Iterator for Heap<T>
where
    T: Default,
{
    type Item = T;

    fn next(&mut self) -> Option<T> {
        //TODO
		None
    }
}

pub struct MinHeap;

impl MinHeap {
    #[allow(clippy::new_ret_no_self)]
    pub fn new<T>() -> Heap<T>
    where
        T: Default + Ord,
    {
        Heap::new(|a, b| a < b)
    }
}

pub struct MaxHeap;

impl MaxHeap {
    #[allow(clippy::new_ret_no_self)]
    pub fn new<T>() -> Heap<T>
    where
        T: Default + Ord,
    {
        Heap::new(|a, b| a > b)
    }
}

#[cfg(test)]
mod tests {
    use super::*;
    #[test]
    fn test_empty_heap() {
        let mut heap = MaxHeap::new::<i32>();
        assert_eq!(heap.next(), None);
    }

    #[test]
    fn test_min_heap() {
        let mut heap = MinHeap::new();
        heap.add(4);
        heap.add(2);
        heap.add(9);
        heap.add(11);
        assert_eq!(heap.len(), 4);
        assert_eq!(heap.next(), Some(2));
        assert_eq!(heap.next(), Some(4));
        assert_eq!(heap.next(), Some(9));
        heap.add(1);
        assert_eq!(heap.next(), Some(1));
    }

    #[test]
    fn test_max_heap() {
        let mut heap = MaxHeap::new();
        heap.add(4);
        heap.add(2);
        heap.add(9);
        heap.add(11);
        assert_eq!(heap.len(), 4);
        assert_eq!(heap.next(), Some(11));
        assert_eq!(heap.next(), Some(9));
        assert_eq!(heap.next(), Some(4));
        heap.add(1);
        assert_eq!(heap.next(), Some(2));
    }
}
```

---

### 本题内容

本练习要求实现一个基本的二叉堆（Binary Heap）数据结构，支持最小堆（MinHeap）和最大堆（MaxHeap）的操作。这是一个非常实用的数据结构，广泛应用于优先队列、堆排序和图算法中的最短路径问题等领域。

### 相关知识点

1. **二叉堆的基本概念**：
   - 二叉堆是一种完全二叉树，通常通过数组来实现。
   - 最小堆（MinHeap）保证根节点是堆中的最小元素；最大堆（MaxHeap）保证根节点是堆中的最大元素。

2. **堆的操作**：
   - **插入（Add）**：插入新元素到堆的末尾，然后调整堆以满足最小堆或最大堆的性质。
   - **删除（Remove）**：移除堆顶元素，通常将最后一个元素移动到堆顶，然后重新调整堆。
   - **查找（Peek）**：查看堆顶元素，不移除。

3. **堆的实现细节**：
   - **上浮（Percolate Up）**：用于插入操作后，确保堆的性质不被破坏。
   - **下沉（Percolate Down）**：用于删除操作后，重新调整堆以保持堆的性质。

### 解题方法

1. **添加元素的方法（Add）**：
    - 将新元素添加到`items`向量的末尾。
    - 使用上浮操作（Percolate Up）调整新元素的位置，确保维持堆的性质。

2. **上浮操作（Percolate Up）**：
    - 如果添加的元素比其父节点小（在最小堆中）或者大（在最大堆中），则与父节点交换位置。
    - 重复这个过程直到元素被放置在正确的位置。

3. **删除元素和获取最小/最大元素**：
    - 将堆顶元素（数组的第一个元素）替换为数组的最后一个元素。
    - 删除数组的最后一个元素。
    - 使用下沉操作（Percolate Down）调整新的堆顶元素的位置，以保持堆的性质。

4. **下沉操作（Percolate Down）**：
    - 比较当前节点与其子节点的值，根据堆的类型（最小堆或最大堆），确定是否需要交换。
    - 如果需要交换，选择值最小（最小堆）或最大（最大堆）的子节点进行交换。
    - 重复这个过程直到当前节点被放置在正确的位置。

### 实现代码

```rust
impl<T> Heap<T>
where
    T: Default + Ord,
{
    pub fn add(&mut self, value: T) {
        self.items.push(value);
        let mut idx = self.count + 1; // 因为我们的堆是从1开始的
        while idx > 1 && (self.comparator)(&self.items[idx], &self.items[self.parent_idx(idx)]) {
            self.items.swap(idx, self.parent_idx(idx));
            idx = self.parent_idx(idx);
        }
        self.count += 1;
    }

    fn percolate_down(&mut self, mut idx: usize) {
        while self.children_present(idx) {
            let sc_idx = self.smallest_child_idx(idx);
            if (self.comparator)(&self.items[sc_idx], &self.items[idx]) {
                self.items.swap(sc_idx, idx);
                idx = sc_idx;
            } else {
                break;
            }
        }
    }

    fn smallest_child_idx(&self, idx: usize) -> usize {
        let l_idx = self.left_child_idx(idx);
        let r_idx = self.right_child_idx(idx);
        if r_idx <= self.count && (self.comparator)(&self.items[r_idx], &self.items[l_idx]) {
            r_idx
        } else {
            l_idx
        }
    }

    fn next(&mut self) -> Option<T> {
        if self.count == 0 {
            None
        } else {
            let result = self.items.swap_remove(1);
            self.count -= 1;
            if self.count > 0 {
                self.items.push(T::default()); // 临时值
                self.items.swap(1, self.count + 1);
                self.items.pop();
                self.percolate_down(1);
            }
            Some(result)
        }
    }
}
```
这段代码实现了堆的基本操作，包括插入新元素、上浮调整、删除元素和下沉调整，以及通过迭代器模式访问元素的功能。

### 代码解释

#### 定义和构造函数
首先，我们定义了一个泛型结构 `Heap`，这个结构用于表示一个二叉堆，可以配置为最小堆或最大堆。它包含以下字段：
- `count`: 堆中元素的数量。
- `items`: 用于存储堆元素的向量。
- `comparator`: 一个函数指针，用于定义元素间的比较逻辑，决定是最小堆还是最大堆。

```rust
pub struct Heap<T>
where
    T: Default,
{
    count: usize,
    items: Vec<T>,
    comparator: fn(&T, &T) -> bool,
}
```

##### 构造函数
构造函数 `new` 接受一个比较函数并初始化堆：
```rust
pub fn new(comparator: fn(&T, &T) -> bool) -> Self {
    Self {
        count: 0,
        items: vec![T::default()], // 初始化时插入一个占位元素，使索引从1开始
        comparator,
    }
}
```
这里，我们向 `items` 向量中插入了一个默认元素，使得元素的索引从1开始，这样父节点和子节点的索引计算更为方便。

#### 基础方法
##### `len` 和 `is_empty`
这两个方法提供了检查堆大小和判空的功能：
```rust
pub fn len(&self) -> usize {
    self.count
}

pub fn is_empty(&self) -> bool {
    self.len() == 0
}
```

#### 添加元素
添加元素时，需要将元素插入到向量的末尾，并执行上浮操作以维护堆的性质：
```rust
pub fn add(&mut self, value: T) {
    self.items.push(value);
    self.count += 1;
    let mut idx = self.count;
    while idx > 1 && (self.comparator)(&self.items[idx], &self.items[self.parent_idx(idx)]) {
        self.items.swap(idx, self.parent_idx(idx));
        idx = self.parent_idx(idx);
    }
}
```
这里，我们先将新元素添加到 `items` 的末尾，然后比较这个元素与其父节点的值，并在需要时通过交换来上浮该元素，直到堆的性质得到满足。

#### 索引计算辅助方法
这些方法帮助我们计算父节点和子节点的索引：
```rust
fn parent_idx(&self, idx: usize) -> usize {
    idx / 2
}

fn left_child_idx(&self, idx: usize) -> usize {
    idx * 2
}

fn right_child_idx(&self, idx: usize) -> usize {
    self.left_child_idx(idx) + 1
}

fn smallest_child_idx(&self, idx: usize) -> usize {
    let left_idx = self.left_child_idx(idx);
    let right_idx = self.right_child_idx(idx);
    if right_idx > self.count || (self.comparator)(&self.items[left_idx], &self.items[right_idx]) {
        left_idx
    } else {
        right_idx
    }
}
```
这里我们定义了获取左右子节点索引的方法和获取最小/最大子节点索引的方法（取决于是最小堆还是最大堆）。

#### 迭代器实现
为了从堆中移除元素，我们实现了 `Iterator` trait：
```rust
impl<T> Iterator for Heap<T>
where
    T: Default,
{
    type Item = T;

    fn next(&mut self) -> Option<T> {
        if self.is_empty() {
            None
        } else {
            let result = self.items.swap_remove(1);
            self.count -= 1;
            if self.count >= 1 {
                self.percolate_down(1);
            }
            Some(result)
        }
    }
}
```
这里的 `next` 方法通过交换根元素与最后一个元素并执行下沉操作来移除根元素，从而维护堆的性质。

通过这样的实现，我们得到了一个功能完整的二叉堆，可以用于

各种需要快速访问最大或最小元素的场景。
