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

本练习的目标是实现合并两个有序的单链表成一个新的有序单链表。这是数据结构中常见的问题，它不仅检验你对单链表的理解，还涉及到合并过程中的指针操作和条件判断，这对于理解更复杂的数据结构和算法非常有帮助。

### 相关知识点：

1. **单链表结构**：单链表是一种线性数据结构，其中的每个元素称为节点，每个节点包含数据和指向下一个节点的指针。
2. **指针安全操作**：使用 `NonNull` 来确保指针非空，这在处理裸指针时提高了安全性。
3. **链表遍历和修改**：通过遍历和修改链表来合并两个链表。

### 解题方法：

1. **创建节点和链表结构**：首先定义节点 (`Node`) 和链表 (`LinkedList`) 的结构，包括基本的增加 (`add`) 和获取节点 (`get`) 方法。
2. **实现合并函数**：`merge` 函数是本题的核心，需要合理使用两个链表的头部节点开始比较，根据大小顺序逐步构建新的链表。
3. **安全考虑**：确保在访问和修改指针时遵循 Rust 的安全原则，特别是在裸指针操作中。

### 代码示例：

这里给出 `merge` 函数的一个基本实现框架：

```rust
pub fn merge(mut list_a: LinkedList<T>, mut list_b: LinkedList<T>) -> LinkedList<T>
where
    T: Ord,
{
    let mut result = LinkedList::new();

    // 用两个可变引用遍历两个链表
    let mut a_curr = list_a.start;
    let mut b_curr = list_b.start;

    while let (Some(a), Some(b)) = (a_curr, b_curr) {
        // 比较当前节点的值决定添加哪个节点到结果链表
        if unsafe { a.as_ref().val } <= unsafe { b.as_ref().val } {
            // 添加 list_a 的当前节点到结果链表
            result.add(unsafe { a.as_ref().val });
            a_curr = unsafe { a.as_ref().next }; // 移动 list_a 的当前节点
        } else {
            // 添加 list_b 的当前节点到结果链表
            result.add(unsafe { b.as_ref().val });
            b_curr = unsafe { b.as_ref().next }; // 移动 list_b 的当前节点
        }
    }

    // 如果一个链表遍历完，将另一个链表剩余部分全部添加到结果链表
    while let Some(a) = a_curr {
        result.add(unsafe { a.as_ref().val });
        a_curr = unsafe { a.as_ref().next };
    }
    while let Some(b) = b_curr {
        result.add(unsafe { b.as_ref().val });
        b_curr = unsafe { b.as_ref().next };
    }

    result
}
```

**注意**：上面的代码需要确保 `Node` 和 `LinkedList` 的方法和属性适用，并且安全地处理了裸指针。实际实现可能需要根据具体的节点和链表定义做调整。此外，还需要处理一些特殊情况，如链表为空等。

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

通过扩展这些知识点和解题方法，学生不仅能解决当前的链表合并问题，还能深入理解 Rust 中的高级编程技巧和内存管理，为处理更复杂的数据结构和算法问题打下坚实的基础。

## 关于链表

Rust 的标准库提供了一些基本的链表结构，如 `std::collections::LinkedList`，但它通常不如自定义的链表结构灵活。在 Rust 中实现自定义链表涉及到一些高级的所有权和借用规则，尤其是在涉及裸指针和安全内存管理时。

### Rust 链表的基本组成

1. **节点定义**：
   - 在 Rust 中，链表通常由节点组成，每个节点至少包含两个元素：存储的值和指向下一个节点的指针（在单链表中）。
   - Rust 中的节点可以使用 `struct` 来定义，其中 `Option<NonNull<Node<T>>>` 通常用于表示指向下一个节点的指针，这种方式比使用 `Box` 更为底层且灵活，但需要更小心地处理内存。

   ```rust
   #[derive(Debug)]
   struct Node<T> {
       val: T,
       next: Option<NonNull<Node<T>>>,
   }
   ```

2. **链表结构**：
   - 链表通常会有一个结构体表示整个链表，这个结构体可能包含指向链表首尾节点的指针和链表的长度等信息。
   - 使用 `Option<NonNull<Node<T>>>` 来安全地处理节点指针，避免空指针和野指针问题。

   ```rust
   #[derive(Debug)]
   struct LinkedList<T> {
       length: u32,
       start: Option<NonNull<Node<T>>>,
       end: Option<NonNull<Node<T>>>,
   }
   ```

### 链表操作

1. **添加节点**：
   - 向链表中添加节点通常涉及修改链表的尾节点指针和可能的头节点指针。
   - 必须正确地处理所有权和借用，尤其是在更新节点指针时。

   ```rust
   impl<T> LinkedList<T> {
       pub fn add(&mut self, obj: T) {
           let mut node = Box::new(Node::new(obj));
           let node_ptr = Some(unsafe { NonNull::new_unchecked(Box::into_raw(node)) });
   
           match self.end {
               None => self.start = node_ptr,
               Some(end_ptr) => unsafe { (*end_ptr.as_ptr()).next = node_ptr },
           }
           self.end = node_ptr;
           self.length += 1;
       }
   }
   ```

2. **合并链表**：
   - 合并两个链表要求遍历两个链表并根据节点值的大小顺序重新链接节点。
   - 实现合并时，应考虑不破坏原有链表结构，合并操作应生成一个新的链表或在其中一个链表上就地修改。

   ```rust
   pub fn merge(list_a: LinkedList<T>, list_b: LinkedList<T>) -> Self {
       // 合并算法实现，可以采用迭代或递归方式
   }
   ```

### 性能和安全性

- Rust 的所有权和借用规则在自定义链表实现中显得尤为重要，因为不正确的内存管理会导致程序崩溃或数据损坏。
- 链表操作的性能可能不如数组和其他连续内存数据结构，特别是在随机访问数据时。但在插入和删除操作中，链表通常能提供更优的性能，尤其是对于大型数据元素。

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

这些知识点涵盖了从内存管理到并发处理，从错误控制到元编程的多个方面，都是在实现和使用 Rust 链表时需要考虑的重要内容。通过理解和应用这些概念，可以有效地构建安全和高效的数据结构。
