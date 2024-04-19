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

### 解题方法步骤

本题要求实现一个二叉堆，二叉堆是一种完全二叉树，通常使用数组来实现。在数组中，给定位置`i`的元素，其左子节点位于`2*i`，右子节点位于`2*i + 1`，父节点位于`i/2`。实现堆的关键功能包括添加元素、删除顶部元素，并保证每次操作后堆的性质依然成立。

#### 1. 初始化
- 堆构造函数初始化一个计数器和一个存储元素的向量。向量的第一个元素通常是一个占位符，用以简化子节点和父节点间的索引计算。

#### 2. 添加元素（`add` 方法）
- 将新元素添加到数组的末尾。
- 使用`bubble_up`方法，将该元素向上移动到正确位置，保持堆的性质。

#### 3. 删除顶部元素（`next` 方法）
- 移除数组中的第一个有效元素（堆顶元素），通常将最后一个元素移动到堆顶，然后使用`bubble_down`方法，将其向下移动到正确位置。

#### 4. 辅助方法
- `parent_idx`, `left_child_idx`, `right_child_idx`: 计算父节点或子节点的索引。
- `children_present`: 检查是否存在子节点。
- `smallest_child_idx`: 返回最小（或最大，取决于是最小堆还是最大堆）子节点的索引。

### 添加的实现代码

以下是根据上述方法步骤的具体实现代码：

```rust
impl<T> Heap<T>
where
    T: Default,
{
    pub fn add(&mut self, value: T) {
        self.items.push(value);
        self.count += 1;
        self.bubble_up(self.count);
    }

    fn bubble_up(&mut self, mut idx: usize) {
        while idx > 1 && (self.comparator)(&self.items[idx], &self.items[self.parent_idx(idx)]) {
            let parent_idx = self.parent_idx(idx);
            self.items.swap(idx, parent_idx);
            idx = parent_idx;
        }
    }

    fn smallest_child_idx(&self, idx: usize) -> usize {
        let left = self.left_child_idx(idx);
        let right = self.right_child_idx(idx);

        if right <= self.count && (self.comparator)(&self.items[right], &self.items[left]) {
            right
        } else {
            left
        }
    }

    fn bubble_down(&mut self, mut idx: usize) {
        while self.children_present(idx) {
            let smallest = self.smallest_child_idx(idx);
            if (self.comparator)(&self.items[smallest], &self.items[idx]) {
                self.items.swap(idx, smallest);
                idx = smallest;
            } else {
                break;
            }
        }
    }

    fn next(&mut self) -> Option<T> {
        if self.is_empty() {
            None
        } else {
            let result = self.items.swap_remove(1); // 移除堆顶元素
            self.count -= 1;
            if !self.items.is_empty() && self.items.len() > 1 {
                // 确保数组中至少有一个元素可以进行下沉
                let last = self.items.pop().unwrap(); // 先存储pop的结果
                self.items.insert(1, last);
                self.bubble_down(1);
            }
            Some(result)
        }
    }
}
```

这段代码实现了堆的基本操作，包括添加新元素、删除顶部元素，并通过`bubble_up`和`bubble_down`方法确保堆的结构和性质。

## 代码解释

### 基本定义与构造函数

```rust
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
```

- `Heap<T>` 是一个泛型结构体，泛型 `T` 必须实现 `Default` 特性，这允许创建默认值。
- `count`: 表示堆中当前元素的数量。
- `items`: 一个向量，用来存储堆中的元素。向量的第一个元素通常是一个默认值，以简化数组索引计算。
- `comparator`: 是一个函数指针，用于比较两个元素的大小。根据这个比较结果来维护堆的结构。这使得同一种数据结构可以用作最小堆或最大堆。

```rust
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
}
```

- `new` 函数是 `Heap` 的构造函数，它接收一个比较函数 `comparator`，初始化 `items` 向量并在其第一个位置放置一个默认元素，`count` 初始化为 0。

### 基本功能

```rust
pub fn len(&self) -> usize {
    self.count
}

pub fn is_empty(&self) -> bool {
    self.len() == 0
}
```

- `len` 返回堆中元素的数量。
- `is_empty` 检查堆是否为空。

### 元素添加与堆调整

```rust
pub fn add(&mut self, value: T) {
    self.items.push(value);
    self.count += 1;
    self.bubble_up(self.count);
}
```

- `add` 方法添加一个元素到堆中。新元素被添加到 `items` 的末尾，然后 `count` 加一。最后调用 `bubble_up` 方法以确保添加后堆的性质得到维护。

```rust
fn bubble_up(&mut self, mut idx: usize) {
    while idx > 1 && (self.comparator)(&self.items[idx], &self.items[self.parent_idx(idx)]) {
        let parent_idx = self.parent_idx(idx);
        self.items.swap(idx, parent_idx);
        idx = parent_idx;
    }
}
```

- `bubble_up` 方法用于将新加入的元素移动到它正确的位置以维护堆的性质。它通过不断与其父节点比较并在必要时交换位置来实现。这个过程会一直进行，直到新元素不再比其父节点更符合堆的条件（例如，在最小堆中更小，或在最大堆中更大），或者该元素已经移动到堆顶。

### 辅助函数

```rust
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
```

- 这些辅助函数用于计算给定索引的父节点索引、左右子节点的索引，以及检查是否存在子节点。这些是维护堆结构和实现堆操作的基础。

继续解释剩余代码中关于堆（`Heap`）的实现，这部分代码涉及创建特定类型的堆（最小堆或最大堆）和实现堆作为迭代器的行为。

### 创建特定类型的堆

```rust
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
```

- 这部分代码为类型 `T`，既有默认值又可排序的情况下，提供创建最小堆和最大堆的方法。
- `new_min()` 和 `new_max()` 是工厂方法，它们使用闭包来定义堆的排序规则：
  - `new_min()` 通过一个闭包 `|a, b| a < b` 实现，使得堆成为一个最小堆，即父节点总是小于其子节点。
  - `new_max()` 使用闭包 `|a, b| a > b`，使得堆成为一个最大堆，即父节点总是大于其子节点。
- 这两种方法都调用了 `Heap` 的 `new` 构造函数，并传入适当的比较函数来实现所需的堆特性。

### 实现 `Iterator` 特性

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
            let result = self.items.swap_remove(1); // 移除堆顶元素
            self.count -= 1;
            if !self.items.is_empty() && self.items.len() > 1 {
                // 确保数组中至少有一个元素可以进行下沉
                let last = self.items.pop().unwrap(); // 先存储pop的结果
                self.items.insert(1, last);
                self.bubble_down(1);
            }
            Some(result)
        }
    }
}
```

- 这部分代码实现了 `Iterator` 特性，允许堆被用作迭代器。每次调用 `next()` 方法都会从堆中移除并返回当前的顶部元素。
- `next()` 方法首先检查堆是否为空，如果为空则返回 `None`。
- 如果堆不为空，使用 `swap_remove(1)` 移除并返回数组中的第一个有效元素（即堆顶）。然后 `count` 减一。
- 接着检查数组是否仍有元素。如果有，取出数组最后一个元素并将其插回到数组开头（堆顶），然后执行 `bubble_down` 方法以重新调整堆，确保维护堆的性质。
- `swap_remove` 方法的使用优化了数组删除元素的效率，但要求后续操作确保堆的结构和排序正确性。

### MinHeap 和 MaxHeap 结构

```rust
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
```

- `MinHeap` 和 `MaxHeap` 是方便的包装器，允许用户直接创建最小堆或最大堆而不需要显式指定比较函数。
- 它们各自的 `new` 方法直接调用 `Heap` 的 `new` 方法，传入适当的比较函数（对于最小堆是 `a < b`，对于最大堆是 `a > b`），从而创建并返回一个配置好的堆实例。

- 这种设计提供了清晰的接口，便于使用和理解。

## 扩展内容

二叉堆是一种非常实用的数据结构，常用于实现优先队列。它能够以对数时间复杂度提供插入和删除最小（或最大）元素的操作，是很多算法和系统功能的基础，如 Dijkstra 的最短路径算法、堆排序、调度算法等。下面，我们探讨二叉堆的一些相关扩展内容：

### 1. 多种类型的堆
除了最常见的二叉最小堆和最大堆外，还有其他类型的堆结构，如：

- **斐波那契堆（Fibonacci Heap）**：在多项式时间内支持一系列堆操作，并能在摊销意义下达到非常低的时间复杂度，特别是对于减小关键字和删除节点操作。
- **二项堆（Binomial Heap）**：由多个满足特定属性的二项树组成，支持快速合并两个堆的操作。
- **四叉堆（Quaternary Heaps）和k叉堆（k-ary Heaps）**：这是二叉堆的一种推广，每个节点可以有k个子节点，它们可能在某些情况下提供更好的常数时间性能。

### 2. 应用
- **优先队列**：在任务调度、事件驱动编程和算法中广泛使用，以确保最重要的任务或数据首先处理。
- **堆排序**：这是一种利用堆数据结构来进行的排序方法，通过构建堆来实现数组的排序。
- **图算法**：在图论中，例如在 Dijkstra 和 Prim 算法中使用堆来处理和更新节点的优先级，以找到最短路径或最小生成树。

### 3. 实现优化
虽然标准的二叉堆已经很高效，但在特定情况下还可以进一步优化：
- **缓存友好性**：由于堆操作依赖于数组的顺序访问，可以通过设计更紧凑的数据结构来优化内存访问模式，提高缓存命中率。
- **并行和并发**：在多核处理器上，可以通过并行算法来优化堆的构建和调整操作，尤其是在处理大量元素时。
- **延迟求值**：在需要的时候才进行堆操作，可以通过懒加载技术推迟堆操作的执行，这在分布式系统中尤为有用。

### 4. Rust 特有的优势和挑战
在 Rust 中实现堆结构，可以利用语言特有的所有权和借用机制来保证在编译时期就消除了很多并发和内存安全的问题。然而，这也意味着实现上需要更仔细地处理所有权和生命周期的问题，特别是在涉及复杂操作如节点交换和内存管理时。

### 结论
二叉堆是一个强大而灵活的工具，可用于解决多种计算问题。通过深入了解其原理和扩展，可以在多种编程环境中高效地应用它们，解决实际问题。Rust 的安全性和性能特性使其成为实现高效数据结构的理想选择。
