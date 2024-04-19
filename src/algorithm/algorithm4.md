# Exercise 104

- Name: ```algorithm4```
- Path: ```exercises/algorithm/algorithm4.rs```
#### Hint: 

No hints this time!


---

```rust,editable
/*
	binary_search tree
	This problem requires you to implement a basic interface for a binary tree
*/
//I AM NOT DONE
use std::cmp::Ordering;
use std::fmt::Debug;

#[derive(Debug)]
struct TreeNode<T>
where
    T: Ord,
{
    value: T,
    left: Option<Box<TreeNode<T>>>,
    right: Option<Box<TreeNode<T>>>,
}

#[derive(Debug)]
struct BinarySearchTree<T>
where
    T: Ord,
{
    root: Option<Box<TreeNode<T>>>,
}

impl<T> TreeNode<T>
where
    T: Ord,
{
    fn new(value: T) -> Self {
        TreeNode {
            value,
            left: None,
            right: None,
        }
    }
}

impl<T> BinarySearchTree<T>
where
    T: Ord,
{
    fn new() -> Self {
        BinarySearchTree { root: None }
    }
    // Insert a value into the BST
    fn insert(&mut self, value: T) {
        //TODO
    }
    // Search for a value in the BST
    fn search(&self, value: T) -> bool {
        //TODO
        true
    }
}

impl<T> TreeNode<T>
where
    T: Ord,
{
    // Insert a node into the tree
    fn insert(&mut self, value: T) {
        //TODO
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_insert_and_search() {
        let mut bst = BinarySearchTree::new();
        assert_eq!(bst.search(1), false);
        
        bst.insert(5);
        bst.insert(3);
        bst.insert(7);
        bst.insert(2);
        bst.insert(4);
        
        assert_eq!(bst.search(5), true);
        assert_eq!(bst.search(3), true);
        assert_eq!(bst.search(7), true);
        assert_eq!(bst.search(2), true);
        assert_eq!(bst.search(4), true);
        assert_eq!(bst.search(1), false);
        assert_eq!(bst.search(6), false);
    }

    #[test]
    fn test_insert_duplicate() {
        let mut bst = BinarySearchTree::new();
        bst.insert(1);
        bst.insert(1);
   
        assert_eq!(bst.search(1), true);
        match bst.root {
            Some(ref node) => {
                assert!(node.left.is_none());
                assert!(node.right.is_none());
            },
            None => panic!("Root should not be None after insertion"),
        }
    }
}    
```

---

### 本题内容

本练习要求学生实现一个基本的二叉搜索树（Binary Search Tree, BST）。通过这个练习，学生将学习如何构建、插入、和搜索一个BST，这是计算机科学中常用的数据结构，用于存储数据以便快速检索。

### 相关知识点

1. **二叉搜索树定义**：
   - 二叉搜索树是一种有序树形结构，其中每个节点包含一个键及其关联值。每个节点的键都大于左子树中任何节点的键，且小于右子树中任何节点的键。

2. **节点插入**：
   - 在BST中插入一个新值时，需要从根节点开始比较，根据比较结果递归地向左或右子树移动，直到找到合适的插入位置。

3. **值搜索**：
   - 搜索操作也是从根节点开始，根据目标值与当前节点值的比较结果递归向左或右子树移动，直到找到目标值或达到叶子节点。

4. **树的遍历**：
   - 二叉树可以通过多种方式遍历，包括前序遍历、中序遍历和后序遍历。中序遍历二叉搜索树可以得到一个有序的数据序列。

### 解题方法

1. **实现 `TreeNode` 构造函数**：
   - 创建一个新的节点，初始化其值，并设置左右子节点为 `None`。

2. **实现 `BinarySearchTree` 的 `insert` 方法**：
   - 如果树为空，则新节点成为根节点。
   - 否则，调用 `TreeNode` 的 `insert` 方法将值插入到适当的位置。

3. **实现 `TreeNode` 的 `insert` 方法**：
   - 比较待插节点的值与当前节点的值，决定是向左子树还是右子树递归插入。
   - 插入操作中，如果遇到相同的值，可以选择忽略或更新。

4. **实现 `search` 方法**：
   - 从根节点开始，递归搜索树，比较节点值，直到找到值或者遍历到叶子节点。

### 代码示例：实现 `insert` 和 `search`

```rust
impl<T> BinarySearchTree<T>
where
    T: Ord,
{
    fn insert(&mut self, value: T) {
        match self.root {
            Some(ref mut node) => node.insert(value),
            None => self.root = Some(Box::new(TreeNode::new(value))),
        }
    }

    fn search(&self, value: T) -> bool {
        self.root.as_ref().map_or(false, |node| node.search(value))
    }
}

impl<T> TreeNode<T>
where
    T: Ord,
{
    fn insert(&mut self, value: T) {
        match value.cmp(&self.value) {
            Ordering::Less => match self.left {
                Some(ref mut left) => left.insert(value),
                None => self.left = Some(Box::new(TreeNode::new(value))),
            },
            Ordering::Greater => match self.right {
                Some(ref mut right) => right.insert(value),
                None => self.right = Some(Box::new(TreeNode::new(value))),
            },
            Ordering::Equal => (), // ignore duplicates or handle them as needed
        }
    }

    fn search(&self, value: T) -> bool {
        match value.cmp(&self.value) {
            Ordering::Equal => true,
            Ordering::Less => self.left.as_ref().map_or(false, |left| left.search(value)),
            Ordering::Greater => self.right.as_ref().map_or(false, |right| right.search(value)),
        }
    }
}
```
这些方法提供了二叉搜索树的基本操作框架，可以根据需要进一步扩展或优化。

二叉搜索树（BST）的基本操作主要包括插入和查找，这些操作通常以递归或迭代的方式实现。上面的实现使用了递归方法，它相对简单直观。除此之外还可以考虑以下几种不同的解法：

### 1. 迭代实现

递归的方法虽然直观，但在某些情况下可能会导致栈溢出，特别是在处理非常大的数据集时。迭代实现可以避免这个问题。

#### 插入操作（迭代方式）

```rust
impl<T> BinarySearchTree<T>
where
    T: Ord,
{
    // 迭代插入值到二叉搜索树
    fn insert(&mut self, value: T) {
        let mut current = &mut self.root;
        while let Some(ref mut node) = current {
            match value.cmp(&node.value) {
                Ordering::Less => current = &mut node.left,
                Ordering::Greater => current = &mut node.right,
                Ordering::Equal => return, // 如果存在相同的值，则不插入
            }
        }
        *current = Some(Box::new(TreeNode::new(value)));
    }
}
```

#### 查找操作（迭代方式）

```rust
impl<T> BinarySearchTree<T>
where
    T: Ord,
{
    // 迭代方式在二叉搜索树中查找值
    fn search(&self, value: T) -> bool {
        let mut current = &self.root;
        while let Some(ref node) = current {
            match value.cmp(&node.value) {
                Ordering::Equal => return true,
                Ordering::Less => current = &node.left,
                Ordering::Greater => current = &node.right,
            }
        }
        false
    }
}
```

### 2. 增加重复值处理

在一些应用中，可能需要在二叉搜索树中存储重复值。一种常见的处理方法是将重复值存储在右子树。

#### 修改插入逻辑以允许重复值

```rust
impl<T> TreeNode<T>
where
    T: Ord,
{
    fn insert(&mut self, value: T) {
        match value.cmp(&self.value) {
            Ordering::Less => {
                if let Some(ref mut left) = self.left {
                    left.insert(value);
                } else {
                    self.left = Some(Box::new(TreeNode::new(value)));
                }
            }
            Ordering::Greater | Ordering::Equal => { // 现在允许重复值，并将它们放在右侧
                if let Some(ref mut right) = self.right {
                    right.insert(value);
                } else {
                    self.right = Some(Box::new(TreeNode::new(value)));
                }
            }
        }
    }
}
```

### 3. 非递归的中序遍历

有时候，为了验证二叉搜索树的正确性，可以实现一个非递归的中序遍历，它应该返回一个有序的序列。

```rust
impl<T> BinarySearchTree<T>
where
    T: Ord,
{
    // 非递归中序遍历
    fn in_order_traversal(&self) -> Vec<&T> {
        let mut result = Vec::new();
        let mut stack = Vec::new();
        let mut current = &self.root;

        while current.is_some() || !stack.is_empty() {
            while let Some(ref node) = current {
                stack.push(node);
                current = &node.left;
            }
            current = stack.pop().unwrap();
            result.push(&current.as_ref().unwrap().value);
            current = &current.as_ref().unwrap().right;
        }

        result
    }
}
```

这些解法展示了如何通过不同方式实现和扩展二叉搜索树的功能。每种方法都有其适用场景和优势，选择哪种实现方式取决于具体的应用需求和环境限制。

## 扩展知识点与解答：

### 扩展知识点

1. **树的平衡性**：
   - 二叉搜索树如果不平衡（即形成长链），会导致操作的效率降低，接近线性查找，从而失去使用树结构的意义。为了解决这个问题，可以使用平衡二叉树，如AVL树或红黑树，它们在每次插入或删除操作后通过旋转和重新着色保持树的平衡。

2. **线索化**：
   - 在二叉树的节点中加入线索（线索是指加上前驱和后继的指针），可以在不使用栈的情况下提高树的遍历效率。

3. **复杂度分析**：
   - 对于理想的平衡二叉搜索树，插入、删除和查找操作的时间复杂度为 O(log n)。如果树不平衡，这些操作的复杂度可能退化为 O(n)。

### 扩展解题方法

1. **红黑树的实现**：
   - 红黑树是一种自平衡的二叉搜索树，它通过节点的颜色（红或黑）来确保从根到叶子的最长的可能路径不会超过最短的可能路径的两倍。这确保了操作的效率。

2. **树的删除操作**：
   - 删除操作比插入和查找更复杂，因为它可能需要重新连接树中的多个节点。处理删除操作时，需要考虑三种情况：删除的节点没有子节点、有一个子节点或有两个子节点。

3. **范型编程**：
   - 在Rust中，可以利用范型来创建可以存储任何数据类型的二叉搜索树。这需要确保数据类型支持比较操作，通常通过约束 `T: Ord` 来实现。

4. **递归与迭代**：
   - 虽然递归是实现树操作的一种直观方法，但在某些情况下，迭代方法（可能使用显式栈）可能更高效，特别是在深度很大的树中，递归可能导致栈溢出。

### 代码示例：实现红黑树（伪代码）

```rust
enum Color {
    Red,
    Black,
}

struct RBNode<T> {
    value: T,
    color: Color,
    left: Option<Box<RBNode<T>>>,
    right: Option<Box<RBNode<T>>>,
    parent: Option<Weak<RBNode<T>>>,
}

impl<T> RBNode<T>
where
    T: Ord,
{
    fn new(value: T) -> Self {
        RBNode {
            value,
            color: Color::Red, // 新节点总是红色
            left: None,
            right: None,
            parent: None,
        }
    }

    fn insert(&mut self, value: T) {
        // 插入逻辑
    }

    fn fixup(&mut self) {
        // 修复红黑树属性
    }
}

struct RBTree<T> {
    root: Option<Box<RBNode<T>>>,
}

impl<T> RBTree<T>
where
    T: Ord,
{
    fn insert(&mut self, value: T) {
        // 插入逻辑
    }
}
```
这个扩展内容涵盖了二叉搜索树的高级应用和优化策略，有助于学生掌握数据结构和算法的深层次理解和应用。
