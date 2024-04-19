# Exercise 101

- Name: ```algorithm1```
- Path: ```exercises/algorithm/algorithm1.rs```
#### Hint: 

No hints this time!


---

```rust,editable
/*
	single linked list merge
	This problem requires you to merge two ordered singly linked lists into one ordered singly linked list
*/
// I AM NOT DONE

use std::fmt::{self, Display, Formatter};
use std::ptr::NonNull;
use std::vec::*;

#[derive(Debug)]
struct Node<T> {
    val: T,
    next: Option<NonNull<Node<T>>>,
}

impl<T> Node<T> {
    fn new(t: T) -> Node<T> {
        Node {
            val: t,
            next: None,
        }
    }
}
#[derive(Debug)]
struct LinkedList<T> {
    length: u32,
    start: Option<NonNull<Node<T>>>,
    end: Option<NonNull<Node<T>>>,
}

impl<T> Default for LinkedList<T> {
    fn default() -> Self {
        Self::new()
    }
}

impl<T> LinkedList<T> {
    pub fn new() -> Self {
        Self {
            length: 0,
            start: None,
            end: None,
        }
    }

    pub fn add(&mut self, obj: T) {
        let mut node = Box::new(Node::new(obj));
        node.next = None;
        let node_ptr = Some(unsafe { NonNull::new_unchecked(Box::into_raw(node)) });
        match self.end {
            None => self.start = node_ptr,
            Some(end_ptr) => unsafe { (*end_ptr.as_ptr()).next = node_ptr },
        }
        self.end = node_ptr;
        self.length += 1;
    }

    pub fn get(&mut self, index: i32) -> Option<&T> {
        self.get_ith_node(self.start, index)
    }

    fn get_ith_node(&mut self, node: Option<NonNull<Node<T>>>, index: i32) -> Option<&T> {
        match node {
            None => None,
            Some(next_ptr) => match index {
                0 => Some(unsafe { &(*next_ptr.as_ptr()).val }),
                _ => self.get_ith_node(unsafe { (*next_ptr.as_ptr()).next }, index - 1),
            },
        }
    }
	pub fn merge(list_a:LinkedList<T>,list_b:LinkedList<T>) -> Self
	{
		//TODO
		Self {
            length: 0,
            start: None,
            end: None,
        }
	}
}

impl<T> Display for LinkedList<T>
where
    T: Display,
{
    fn fmt(&self, f: &mut Formatter) -> fmt::Result {
        match self.start {
            Some(node) => write!(f, "{}", unsafe { node.as_ref() }),
            None => Ok(()),
        }
    }
}

impl<T> Display for Node<T>
where
    T: Display,
{
    fn fmt(&self, f: &mut Formatter) -> fmt::Result {
        match self.next {
            Some(node) => write!(f, "{}, {}", self.val, unsafe { node.as_ref() }),
            None => write!(f, "{}", self.val),
        }
    }
}

#[cfg(test)]
mod tests {
    use super::LinkedList;

    #[test]
    fn create_numeric_list() {
        let mut list = LinkedList::<i32>::new();
        list.add(1);
        list.add(2);
        list.add(3);
        println!("Linked List is {}", list);
        assert_eq!(3, list.length);
    }

    #[test]
    fn create_string_list() {
        let mut list_str = LinkedList::<String>::new();
        list_str.add("A".to_string());
        list_str.add("B".to_string());
        list_str.add("C".to_string());
        println!("Linked List is {}", list_str);
        assert_eq!(3, list_str.length);
    }

    #[test]
    fn test_merge_linked_list_1() {
		let mut list_a = LinkedList::<i32>::new();
		let mut list_b = LinkedList::<i32>::new();
		let vec_a = vec![1,3,5,7];
		let vec_b = vec![2,4,6,8];
		let target_vec = vec![1,2,3,4,5,6,7,8];
		
		for i in 0..vec_a.len(){
			list_a.add(vec_a[i]);
		}
		for i in 0..vec_b.len(){
			list_b.add(vec_b[i]);
		}
		println!("list a {} list b {}", list_a,list_b);
		let mut list_c = LinkedList::<i32>::merge(list_a,list_b);
		println!("merged List is {}", list_c);
		for i in 0..target_vec.len(){
			assert_eq!(target_vec[i],*list_c.get(i as i32).unwrap());
		}
	}
	#[test]
	fn test_merge_linked_list_2() {
		let mut list_a = LinkedList::<i32>::new();
		let mut list_b = LinkedList::<i32>::new();
		let vec_a = vec![11,33,44,88,89,90,100];
		let vec_b = vec![1,22,30,45];
		let target_vec = vec![1,11,22,30,33,44,45,88,89,90,100];

		for i in 0..vec_a.len(){
			list_a.add(vec_a[i]);
		}
		for i in 0..vec_b.len(){
			list_b.add(vec_b[i]);
		}
		println!("list a {} list b {}", list_a,list_b);
		let mut list_c = LinkedList::<i32>::merge(list_a,list_b);
		println!("merged List is {}", list_c);
		for i in 0..target_vec.len(){
			assert_eq!(target_vec[i],*list_c.get(i as i32).unwrap());
		}
	}
}
```

---

### 本题内容

本练习要求学生实现一个函数，用于合并两个已排序的单向链表。目标是创建一个新的链表，其中包含两个输入链表中所有元素的有序合并。这是数据结构中常见的一个问题，理解其解决方法对于掌握链表操作非常重要。

### 相关知识点

1. **单向链表**：一种数据结构，其中的每个节点包含数据和指向链表中下一个节点的指针。这种结构允许有效地在序列的一端添加元素，但访问或搜索元素通常需要从头部开始遍历。

2. **链表节点的管理**：包括创建节点、管理节点的生命周期（在 Rust 中涉及所有权和生命周期管理），以及节点的连接和断开。

3. **合并有序列表**：将两个已排序的列表合并成一个新的有序列表，这是排序算法和数据结构中的一个重要操作，常见于归并排序算法中。

4. **所有权和借用**：Rust 独特的所有权系统管理内存，确保安全地使用数据，特别是在复杂结构如链表的创建和修改中。

### 解题方法

#### 1. 迭代方法

**迭代方法**的主要优点是空间效率高，因为它只需要常数级额外空间来存储几个指针。下面是实现的步骤：

- 初始化两个指针，分别指向两个链表的头部。
- 创建一个虚拟头节点，这将简化边界条件的处理。
- 使用一个迭代指针从头开始遍历两个链表，每次迭代中比较两个链表当前节点的值。
- 将较小值的节点连接到结果链表的当前节点，并移动较小值节点的指针到下一个。
- 继续迭代直到一个链表为空。
- 将非空链表的剩余部分链接到结果链表的末尾。
- 返回虚拟头节点的下一个节点，即合并后的链表的头部。

#### 2. 递归方法

**递归方法**在概念上更简单，代码通常更短，但可能会因为深度递归而导致堆栈溢出。其实现方式如下：

- 如果任一链表为空，返回另一链表。
- 比较两个链表的头节点：
  - 如果第一个链表的头节点值较小，则将这个节点的 `next` 设置为其余节点和第二个链表合并后的结果。
  - 反之，如果第二个链表的头节点值较小，则操作类似，只是目标链表变为第二个。
- 返回当前选择的头节点，这样递归调用会构建整个链表。

### 示例实现：迭代方法

```rust
impl<T: PartialOrd + Copy + Display> LinkedList<T> {
    /// 将两个排序的链表合并为一个排序的链表。
    pub fn merge(list_a: LinkedList<T>, list_b: LinkedList<T>) -> Self {
        // 创建一个新的空链表作为合并后的结果
        let mut result = Self::new();
        
        // 初始化两个变量来分别跟踪两个输入链表的当前节点
        let mut node_a = list_a.start;
        let mut node_b = list_b.start;

        // 继续迭代，直到其中一个链表完全遍历完毕
        while let (Some(a), Some(b)) = (node_a, node_b) {
            unsafe {
                // 比较两个链表当前节点的值，并将较小的节点的值添加到结果链表中
                if (*a.as_ptr()).val <= (*b.as_ptr()).val {
                    result.add((*a.as_ptr()).val);
                    // 移动到链表a的下一个节点
                    node_a = (*a.as_ptr()).next;
                } else {
                    result.add((*b.as_ptr()).val);
                    // 移动到链表b的下一个节点
                    node_b = (*b.as_ptr()).next;
                }
            }
        }

        // 一旦一个链表被完全遍历，将另一个链表的剩余部分添加到结果链表中
        while let Some(a) = node_a {
            unsafe {
                result.add((*a.as_ptr()).val);
                node_a = (*a.as_ptr()).next;
            }
        }
        while let Some(b) = node_b {
            unsafe {
                result.add((*b.as_ptr()).val);
                node_b = (*b.as_ptr()).next;
            }
        }

        // 返回合并后的新链表
        result
    }
}
```

### 代码解释

1. **泛型约束** (`<T: PartialOrd + Copy + Display>`): 这确保了 `T` 类型的元素可以被比较（使用 `<` 或 `<=`）、复制（而不仅仅是移动，这对于不可变数据类型很重要），并且可以显示（例如在打印或日志中）。

2. **创建结果链表**: 一个新的空 `LinkedList` 被创建，用于存储合并后的节点。

3. **主循环** (`while let (Some(a), Some(b)) = (node_a, node_b)`): 这个循环持续进行，直到 `node_a` 或 `node_b` 其中之一为 `None`。在循环内部，使用 `unsafe` 块来解引用裸指针。这是必需的，因为 `NonNull` 本身并不保证所指向的内存是有效的，但它确保了不会是 `null`。

4. **添加较小节点的值**: 根据节点的值进行比较，并添加较小的值到结果链表中，然后将相应链表的指针移动到下一个节点。

5. **处理剩余的节点**: 两个 `while let` 循环处理未被完全遍历的链表，将剩余的节点依次添加到结果链表中。

通过这种方式，`merge` 函数有效地将两个链表合并成一个有序链表，同时保证了在 Rust 中操作的安全性。这个实现不仅确保了代码的正确性，也考虑了效率和安全性，是处理此类问题的一个典型范例。

### 示例实现：递归方法

```rust
impl<T: PartialOrd + Copy + Display> LinkedList<T> {
    /// 将两个排序的链表合并为一个排序的链表。
    pub fn merge(list_a: LinkedList<T>, list_b: LinkedList<T>) -> Self {
        /// 递归合并两个链表节点。
        fn merge_recursive<T: PartialOrd + Copy + Display>(node_a: Option<NonNull<Node<T>>>, node_b: Option<NonNull<Node<T>>>) -> Option<NonNull<Node<T>>> {
            match (node_a, node_b) {
                (None, _) => node_b,  // 如果链表a为空，返回当前链表b的节点。
                (_, None) => node_a,  // 如果链表b为空，返回当前链表a的节点。
                (Some(a), Some(b)) => unsafe {  // 如果两个链表都不为空，比较节点值。
                    if (*a.as_ptr()).val <= (*b.as_ptr()).val {
                        // 如果a节点的值小于等于b节点的值，设置a节点的next为递归合并后的结果。
                        (*a.as_ptr()).next = merge_recursive((*a.as_ptr()).next, Some(b));
                        Some(a)  // 返回节点a作为合并链表的当前节点。
                    } else {
                        // 如果b节点的值小于a节点的值，设置b节点的next为递归合并后的结果。
                        (*b.as_ptr()).next = merge_recursive(Some(a), (*b.as_ptr()).next);
                        Some(b)  // 返回节点b作为合并链表的当前节点。
                    }
                },
            }
        }

        // 开始递归合并，并从返回的节点开始构建新链表。
        let start = merge_recursive(list_a.start, list_b.start);
        let mut merged_list = Self::new();  // 创建一个新的空链表作为合并后的结果。
        merged_list.start = start;  // 设置新链表的起始节点。

        // 可选：如果需要维护链表的长度和结束节点
        // 此代码假设合并过程中节点没有被消耗，并统计剩余的节点数
        let mut current = merged_list.start;
        let mut end = None;
        let mut length = 0;
        while let Some(node) = current {
            unsafe {
                end = Some(node);  // 更新末尾节点。
                current = (*node.as_ptr()).next;  // 移动到下一个节点。
                length += 1;  // 链表长度加一。
            }
        }
        merged_list.end = end;  // 设置新链表的结束节点。
        merged_list.length = length;  // 设置新链表的长度。

        merged_list  // 返回合并后的链表。
    }
}
```

### 代码解释

1. **泛型约束**：`<T: PartialOrd + Copy + Display>` 确保元素类型 `T` 可以进行比较、复制和显示。这对于合并排序链表是必要的，因为需要比较元素以决定其顺序。

2. **递归函数 `merge_recursive`**：这是一个私有函数，用于递归地合并两个链表的节点。递归的基本案例处理了一个或两个链表为空的情况。如果两个链表都非空，它将比较头节点的值，并递归地合并剩余部分。

3. **构建结果链表**：在递归完成后，使用从 `merge_recursive` 返回的节点作为新链表的起始节点，然后通过遍历这些节点来构建完整的链表，并计算长度和更新结束节点。

这种递归方法直观且易于理解，尤其适用于链表的操作，因为它自然地遵循链表的节点链接结构。不过，需要注意的是，递归可能会在处理非常长的链表时导致堆栈溢出。

---

以下是更多详细分析和解释，包括数据结构定义、辅助函数、和特定实现方法的解释。

### 数据结构定义

#### Node 结构体

`Node` 是链表的基本组成单元，它包含存储在链表中的值（`val`）和指向链表中下一个节点的指针（`next`）。

```rust
#[derive(Debug)]
struct Node<T> {
    val: T,
    next: Option<NonNull<Node<T>>>,
}
```

- `val`: 存储节点的数据。
- `next`: 是一个 `Option<NonNull<Node<T>>>` 类型，用于安全地指向链表中的下一个节点。`NonNull` 保证指针不为空，而 `Option` 允许这个指针在逻辑上可以是 `None`（比如在链表末尾）。

#### LinkedList 结构体

`LinkedList` 表示整个链表，包含链表的长度和指向链表头尾节点的指针。

```rust
#[derive(Debug)]
struct LinkedList<T> {
    length: u32,
    start: Option<NonNull<Node<T>>>,
    end: Option<NonNull<Node<T>>>,
}
```

- `length`: 链表的长度，用于快速访问链表大小信息。
- `start`: 指向链表的第一个节点。
- `end`: 指向链表的最后一个节点。

### 核心方法实现

#### new 方法

创建一个空的 `LinkedList`。

```rust
impl<T> LinkedList<T> {
    pub fn new() -> Self {
        Self {
            length: 0,
            start: None,
            end: None,
        }
    }
}
```

#### add 方法

向链表的末尾添加一个新节点。

```rust
pub fn add(&mut self, obj: T) {
    let mut node = Box::new(Node::new(obj));
    node.next = None;
    let node_ptr = Some(unsafe { NonNull::new_unchecked(Box::into_raw(node)) });

    match self.end {
        None => self.start = node_ptr,
        Some(end_ptr) => unsafe { (*end_ptr.as_ptr()).next = node_ptr },
    }
    self.end = node_ptr;
    self.length += 1;
}
```

- 创建一个新的 `Node` 实例并将其转化为裸指针。
- 如果链表为空，将 `start` 和 `end` 都指向新节点。
- 如果不为空，将当前末尾节点的 `next` 指向新节点，并更新 `end` 指针。

#### get 方法

获取链表中指定索引位置的元素。

```rust
pub fn get(&mut self, index: i32) -> Option<&T> {
    self.get_ith_node(self.start, index)
}
```

- 调用 `get_ith_node` 方法来递归地寻找指定索引的节点。

#### get_ith_node 方法

递归地获取链表中指定索引的节点。

```rust
fn get_ith_node(&mut self, node: Option<NonNull<Node<T>>>, index: i32) -> Option<&T> {
    match node {
        None => None,
        Some(next_ptr) => match index {
            0 => Some(unsafe { &(*next_ptr.as_ptr()).val }),
            _ => self.get_ith_node(unsafe { (*next_ptr.as_ptr()).next }, index - 1),
        },
    }
}
```

- 如果节点是 `None` 或索引为 0，直接返回节点的值。
- 否则递归地在下一个节点查找。

### 合并两个链表的方法

#### merge 方法

合并两个已排序的链表。

```rust
pub fn merge(list_a: LinkedList<T>, list_b: LinkedList<T>) -> Self {
    // 实现细节
}
```

- 逐一比较两个链表的头部，将较小的节点添加到新链表中。
- 当一个链表为空时，将另一个链表的余部分直接链接到新链表的尾部。由于具体的合并逻辑未在你的原始代码中提供，这里我们只能大概描述这一过程。正确的合并操作需要遵循迭代或递归的逻辑，确保新链表保持有序。

### 特殊方法实现

#### Display Trait for LinkedList

实现 `Display` 特征，使得链表可以被更人性化地打印出来，便于调试和展示链表内容。

```rust
impl<T> Display for LinkedList<T>
where
    T: Display,
{
    fn fmt(&self, f: &mut Formatter) -> fmt::Result {
        let mut current = self.start;
        while let Some(node) = current {
            unsafe {
                write!(f, "{} -> ", (*node.as_ptr()).val)?;
                current = (*node.as_ptr()).next;
            }
        }
        write!(f, "None")
    }
}
```

这个实现使用 `Formatter` 来连续写入每个节点的值，并在每个节点后加上箭头 `->` 表示指向下一个节点。链表末尾写入 `"None"` 表明链表结束。

#### Display Trait for Node

类似地，也为节点实现 `Display` 特征，以便单独打印节点时能清楚地显示节点的值和其后继的状态。

```rust
impl<T> Display for Node<T>
where
    T: Display,
{
    fn fmt(&self, f: &mut Formatter) -> fmt::Result {
        match self.next {
            Some(next) => write!(f, "{}, next -> ", self.val),
            None => write!(f, "{}, next -> None", self.val),
        }
    }
}
```

此实现中，如果节点有后继节点，则显示该节点的值和指向下一个节点的文本，如果没有后继节点，则显示 `"None"` 表明这是链表的末尾。

### 总结

通过这种全面的方法，我们不仅定义了链表和节点的基本结构，还实现了添加新节点、获取特定节点、显示链表和节点以及合并两个链表的关键功能。这为处理更复杂的数据结构问题奠定了坚实的基础，并深入展示了 Rust 语言在管理安全、高效数据结构方面的强大功能。每个函数和方法的实现都考虑到了 Rust 的安全性要求，特别是在处理裸指针和可变借用时。

---

下面将详细讲解代码中涉及的每个部分，包括 Box 的使用、unsafe 代码块的安全考虑、以及 Rust 中的模式匹配（match）、`Display` 特征等。

### Box 构建和节点管理

#### Box 的使用

```rust
let mut node = Box::new(Node::new(obj));
```

在 Rust 中，`Box<T>` 是一种智能指针，用于在堆上分配空间。在这里，它被用来创建一个新的链表节点。使用 `Box` 是因为链表的节点需要在运行时动态管理，`Box` 提供了简单的方式来处理堆上的数据。

#### 转换为裸指针

```rust
let node_ptr = Some(unsafe { NonNull::new_unchecked(Box::into_raw(node)) });
```

- `Box::into_raw(node)` 将 `Box` 所有权转换为一个裸指针（raw pointer），这使得节点的所有权转移，避免了 Rust 默认的内存自动清理机制，允许我们手动管理内存。
- `NonNull::new_unchecked` 是一个包装裸指针的安全包装器，它保证了指针不是空（non-null），这对性能优化有帮助，因为编译器可以假设指针总是有效的。

### 添加节点到链表

```rust
match self.end {
    None => self.start = node_ptr,
    Some(end_ptr) => unsafe { (*end_ptr.as_ptr()).next = node_ptr },
}
self.end = node_ptr;
self.length += 1;
```

- 使用 `match` 语句来处理链表的尾节点是否存在。如果链表为空（`self.end` 是 `None`），则新节点成为链表的起始和结束节点。否则，将新节点链接到原来的尾节点后面。
- `unsafe { (*end_ptr.as_ptr()).next = node_ptr }` 在这里需要使用 `unsafe` 代码块，因为 `.as_ptr()` 返回一个裸指针，需要手动保证访问安全。这里的操作是将新节点设置为当前尾节点的 `next` 节点。

### 获取节点

```rust
pub fn get(&mut self, index: i32) -> Option<&T> {
    self.get_ith_node(self.start, index)
}
```

这个函数是链表获取元素的接口，它调用 `get_ith_node` 以递归方式查找特定索引的节点。

### 递归查找节点

```rust
fn get_ith_node(&mut self, node: Option<NonNull<Node<T>>>, index: i32) -> Option<&T> {
    match node {
        None => None,
        Some(next_ptr) => match index {
            0 => Some(unsafe { &(*next_ptr.as_ptr()).val }),
            _ => self.get_ith_node(unsafe { (*next_ptr.as_ptr()).next }, index - 1),
        },
    }
}
```

- 第一个 `match` 处理当前节点是否存在，如果不存在（即链表结尾），则返回 `None`。
- 第二个 `match` 判断是否达到了所需的索引，如果 `index` 为 0，说明已找到目标节点，返回其值的引用。
- 如果不是目标索引，则递归调用 `get_ith_node`，索引减一，直到索引为 0。

### 实现 Display 特征为 LinkedList

```rust
impl<T> Display for LinkedList<T>
where
    T: Display,
{
    fn fmt(&self, f: &mut Formatter) -> fmt::Result {
        let mut current = self.start;
        while let Some(node) = current {
            unsafe {
                write!(f, "{} -> ", (*node.as_ptr()).val)?;
                current = (*node.as_ptr()).next;
            }
        }
        write!(f, "None")
    }
}
```

#### 解释：

- `impl<T> Display for LinkedList<T>`：这里定义了一个泛型实现，要求泛型 `T` 必须实现 `Display` 特征，这使得链表中的元素可以被打印。

- `fn fmt(&self, f: &mut Formatter) -> fmt::Result`：这是 `Display` 特征必须实现的函数，它用于将数据格式化到提供的格式化器 `f`。

- `let mut current = self.start;`：初始化一个变量 `current` 来遍历链表，从头节点开始。

- `while let Some(node) = current`：这是一个 `while let` 循环，只要 `current` 是 `Some(node)`，循环就会继续。这允许我们遍历整个链表。

- `unsafe { write!(f, "{} -> ", (*node.as_ptr()).val)?; }`：在 `unsafe` 代码块中，我们使用 `node.as_ptr()` 获取节点的裸指针，并通过解引用打印节点值。由于这里涉及裸指针，必须确保这些操作的安全性。

- `current = (*node.as_ptr()).next;`：更新 `current` 为下一个节点，继续遍历。

- `write!(f, "None")`：在链表末尾输出 "None"，表示链表已结束。

### 实现 Display 特征为 Node

```rust
impl<T> Display for Node<T>
where
    T: Display,
{
    fn fmt(&self, f: &mut Formatter) -> fmt::Result {
        match self.next {
            Some(next) => write!(f, "{}, next -> ", self.val),
            None => write!(f, "{}, next -> None", self.val),
        }
    }
}
```

#### 解释：

- 这里为节点 `Node` 实现 `Display` 特征，同样要求 `T` 实现 `Display`。

- `fn fmt(&self, f: &mut Formatter) -> fmt::Result`：同样是必需实现的格式化函数。

- `match self.next`：通过 `match` 语句检查节点的 `next` 指针是否为空，这决定了如何打印节点的后续部分。

- `Some(next) => write!(f, "{}, next -> ", self.val)`：如果 `next` 不为空，打印当前节点的值后跟 "next ->"。

- `None => write!(f, "{}, next -> None", self.val)`：如果 `next` 为空，打印当前节点的值后跟 "next -> None"，表示这是链表的末尾节点。

---

### 扩展知识点：

1. **链表深入理解**：
   - 单链表和双链表的区别。
   - 如何在链表中执行高效的插入和删除操作。
   - 链表的循环版本和非循环版本的差异。

2. **更多的链表操作**：
   - 反转链表：一个常见的技术面试问题，涉及到改变链表中节点的指向。
   - 查找链表中的中间节点：可以使用快慢指针的策略来解决。
   - 检测链表中的环：使用快慢指针来检测链表是否存在环。

3. **泛型编程**：
   - 如何在 Rust 中使用泛型来构建可复用的数据结构。
   - 理解和应用生命周期以确保引用的有效性和安全性。

4. **内存管理**：
   - 理解 Rust 的所有权、借用和生命周期是如何帮助管理内存的。
   - 学习如何在不使用 `unsafe` 代码的情况下管理复杂的内存布局。

### 扩展解题方法：

1. **递归合并**：
   - 尝试使用递归方法来合并两个链表，这种方法可以简化代码，但需要注意栈溢出的风险。
   - 在递归方法中，基本情况是其中一个链表为空，递归步骤是比较头节点的值并递归地合并剩余部分。

2. **迭代合并**：
   - 使用迭代方法通过循环遍历两个链表来合并它们，这是更加实用和高效的方法，因为它避免了递归可能引起的栈溢出问题。
   - 使用哨兵节点（dummy node）来简化边界条件的处理，这样可以统一在循环中的插入逻辑。

3. **优化内存使用**：
   - 在合并链表时，尝试重用输入链表的节点而不是创建新节点，这可以减少内存分配和释放的开销。
   - 考虑在链表节点中使用 `Box` 或 `Option<NonNull>` 来确保空间使用和安全性的最优化。

4. **单元测试**：
   - 编写多个单元测试来验证各种情况，如两个空链表的合并、一个空链表和一个非空链表的合并、两个具有相同元素的链表的合并等。
   - 使用边界值测试和随机化测试来确保代码的健壮性和正确性。

## 关于`Option<NonNull<Node<T>>>`

在 Rust 中，使用 `Option<NonNull<Node<T>>>` 来处理链表节点的指针是一种常见的做法，这种方式旨在安全地处理可能的空指针和野指针问题，同时保持对底层内存的有效控制。下面详细解释这种方法的关键组件：

### Option

`Option` 是 Rust 中的一个枚举，用于表示可能存在或不存在的值。它的两个变体是：
- `Some(T)`: 表示有一个类型为 `T` 的值存在。
- `None`: 表示不存在值。

在链表的上下文中，使用 `Option` 包裹指针（或其他类型的引用）可以安全地处理链表的末尾，即没有下一个节点的情况。通过这种方式，你可以在尝试访问下一个节点之前检查是否存在该节点，从而避免空指针访问的错误。

### NonNull

`NonNull<T>` 是 Rust 标准库中的一个类型，它用于封装原始非空指针。`NonNull` 提供了一些优势：
- 它保证包裹的指针永远不为空。
- 它是协变的，这意味着如果你有一个 `NonNull<T>`，你可以转换成 `NonNull<U>`，其中 `U` 是 `T` 的超类型（parent type）。
- `NonNull` 不自动实现 `Send` 和 `Sync`，使得在多线程环境中的使用必须更加小心，以确保线程安全。

使用 `NonNull` 可以向编译器表明你已经确保了指针不为空，这可以优化某些编译器检查和操作。

### 结合使用 Option 和 NonNull

将 `NonNull` 包裹在 `Option` 中使用，可以结合两者的优势：安全地处理可能的空指针（通过 `Option`）和保证当指针存在时它绝不为空（通过 `NonNull`）。这在实现如链表这类数据结构时特别有用，因为你经常需要检查节点是否指向另一个节点。

例如，在链表中添加新节点时，你可能会这样做：
```rust
let mut node = Box::new(Node::new(obj));
let node_ptr = Some(unsafe { NonNull::new_unchecked(Box::into_raw(node)) });

match self.end {
    None => self.start = node_ptr,
    Some(end_ptr) => unsafe {
        (*end_ptr.as_ptr()).next = node_ptr
    },
}
self.end = node_ptr;
```
这里，`Box::into_raw(node)` 将节点的所有权转换为原始指针，`NonNull::new_unchecked()` 将这个原始指针封装为 `NonNull`，最后使用 `Option` 来处理链表的尾部可能为空的情况。

### 安全注意事项

尽管 `NonNull` 和 `Option` 提供了处理裸指针的安全封装，使用它们时仍需要小心。特别是使用 `unsafe` 代码块时，你需要确保所有的不变量得到保证，比如确保指针指向的内存在使用时是有效的，这通常意味着你需要在某处保留这段内存的所有权，直到你确定不再需要它为止。这是为了防止悬垂指针或双重释放等危险的内存错误。

## 其它相关知识点

在深入探讨 Rust 中的链表实现时，除了前面提到的 `Option<NonNull<Node<T>>>` 结构之外，还有许多其他的关键知识点和概念是值得理解的。这些知识点帮助构建安全、高效且健壮的链表数据结构：

### 1. 内存安全和所有权
- **所有权管理**：Rust 的所有权系统确保在链表中每个节点的内存只有一个有效的引用，防止内存泄漏和野指针问题。
- **`Box<T>`**：Rust 中的 `Box` 是一个智能指针，用于在堆上分配内存。在链表中使用 `Box` 可以拥有其指向的数据，并且能够自动处理数据的释放。

### 2. 生命期和借用检查
- **生命周期注解**：确保链表中的引用不会比它们指向的数据活得更长。生命周期注解帮助编译器理解这些引用的有效期，防止悬垂引用。
- **可变与不可变借用**：Rust 通过区分可变借用和不可变借用来保障数据在并发访问时的安全性。链表的操作如添加或删除节点通常需要可变借用。

### 3. 错误处理
- **使用 Result 和 Option**：链表操作中可能失败的场景（例如尝试从空链表中删除元素）可以通过返回 `Option` 或 `Result` 类型来安全处理。

### 4. 迭代器
- **实现迭代器**：为链表实现 `IntoIterator`, `Iterator`, 和 `DoubleEndedIterator` 等迭代器特质可以提高链表的灵活性和可用性。
- **迭代器无效化**：在迭代链表时修改链表（如添加或删除节点）可能会导致迭代器无效化，这需要特别注意。

### 5. 并发和线程安全
- **使用 Arc 和 Mutex**：在多线程环境中共享或修改链表时，可以使用 `Arc`（原子引用计数）和 `Mutex`（互斥锁）来确保线程安全。

### 6. 高级特性
- **条件编译**：根据目标平台或配置条件使用 `cfg` 属性来包含或排除特定的实现。
- **特质对象**：通过定义特质和相关的方法，可以提高链表的模块化和复用性。

### 7. 元编程
- **宏**：使用宏来减少重复代码，例如自动实现某些链表操作或特质。
