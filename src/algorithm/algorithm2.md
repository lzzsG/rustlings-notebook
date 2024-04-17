# Exercise 102

- Name: ```algorithm2```
- Path: ```exercises/algorithm/algorithm2.rs```
#### Hint: 

No hints this time!


---



```rust,editable
/*
	double linked list reverse
	This problem requires you to reverse a doubly linked list
*/
// I AM NOT DONE

use std::fmt::{self, Display, Formatter};
use std::ptr::NonNull;
use std::vec::*;

#[derive(Debug)]
struct Node<T> {
    val: T,
    next: Option<NonNull<Node<T>>>,
    prev: Option<NonNull<Node<T>>>,
}

impl<T> Node<T> {
    fn new(t: T) -> Node<T> {
        Node {
            val: t,
            prev: None,
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
        node.prev = self.end;
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
	pub fn reverse(&mut self){
		// TODO
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
    fn test_reverse_linked_list_1() {
		let mut list = LinkedList::<i32>::new();
		let original_vec = vec![2,3,5,11,9,7];
		let reverse_vec = vec![7,9,11,5,3,2];
		for i in 0..original_vec.len(){
			list.add(original_vec[i]);
		}
		println!("Linked List is {}", list);
		list.reverse();
		println!("Reversed Linked List is {}", list);
		for i in 0..original_vec.len(){
			assert_eq!(reverse_vec[i],*list.get(i as i32).unwrap());
		}
	}

	#[test]
	fn test_reverse_linked_list_2() {
		let mut list = LinkedList::<i32>::new();
		let original_vec = vec![34,56,78,25,90,10,19,34,21,45];
		let reverse_vec = vec![45,21,34,19,10,90,25,78,56,34];
		for i in 0..original_vec.len(){
			list.add(original_vec[i]);
		}
		println!("Linked List is {}", list);
		list.reverse();
		println!("Reversed Linked List is {}", list);
		for i in 0..original_vec.len(){
			assert_eq!(reverse_vec[i],*list.get(i as i32).unwrap());
		}
	}
}
```

---

### 本题内容

本练习的目标是实现一个双向链表的反转功能。通过这个练习，学生将学习如何操作链表中的节点指针来实现复杂的链表操作，如反转链表。这项技能在数据结构和算法的学习中非常重要，尤其是在处理链表数据结构时。

### 相关知识点：
- **双向链表结构**：双向链表允许从两个方向遍历，每个节点都有指向前一个和后一个节点的指针。
- **`Option<NonNull<T>>` 的使用**：在 Rust 中，`NonNull` 用于表示非空的裸指针，与 `Option` 结合使用时，可以有效地表示可能存在或不存在的非空指针，这在管理链表节点时非常有用。
- **内存安全**：使用 `unsafe` 代码块来处理裸指针，需要确保操作的安全性，避免出现悬挂指针和无效引用。
- **所有权和借用**：在 Rust 中，所有权和借用规则确保了内存安全，不过在处理裸指针时，需要手动管理内存。

### 解题方法：
1. **初始化节点和链表**：首先创建 `Node` 和 `LinkedList` 结构体，为它们实现构造函数和基本操作方法。
2. **添加节点**：实现 `add` 方法向链表添加节点，确保更新节点的前驱和后继指针。
3. **反转链表**：
   - 创建 `reverse` 方法。
   - 遍历链表，交换每个节点的 `next` 和 `prev` 指针。
   - 更新链表的 `start` 和 `end` 指针，以反映新的首尾节点。
4. **测试**：编写测试用例以验证链表的反转功能是否正确实现。

### 代码示例：
这是反转双向链表的 `reverse` 方法的简化实现：

```rust
pub fn reverse(&mut self) {
    let mut current = self.start;
    let mut temp = None;
    while let Some(mut node) = current {
        // 交换 next 和 prev
        unsafe {
            temp = node.as_mut().next;
            node.as_mut().next = node.as_mut().prev;
            node.as_mut().prev = temp;
        }
        // 更新 current 到下一个节点，实际上是前一个节点，因为已经交换
        current = temp;
    }
    // 交换链表的起始和结束指针
    std::mem::swap(&mut self.start, &mut self.end);
}
```

此代码中使用了 `unsafe` 块，因为它直接操作裸指针。这种方式在处理双向链表时很常见，但需要特别注意安全性，避免出现内存错误。

### 扩展知识点：

1. **裸指针的使用**：
   - 在 Rust 中，裸指针（raw pointers）分为两种：`*const T` 和 `*mut T`，分别表示不可变和可变的指针。
   - 裸指针提供了绕过 Rust 安全借用规则的能力，允许直接内存访问，但使用时不提供任何自动的内存安全保证。
   - 在使用裸指针时，通常需要用 `unsafe` 块标记，这要求开发者必须手动保证代码的安全性。

2. **双向链表与单链表的区别**：
   - 双向链表的节点包含两个指针，分别指向前一个节点和后一个节点，这增加了额外的内存开销，但提供了双向遍历的能力。
   - 反转双向链表相对于单链表更为直接，因为节点已经包含指向前一个节点的指针，可以在单次遍历中完成反转。

3. **内存管理**：
   - 使用 `Box<T>` 来管理节点可以确保节点在堆上分配内存，并且在 `Box` 被销毁时自动释放内存。
   - 当手动处理指针如在双向链表中时，需要注意正确地维护引用以避免内存泄漏或悬挂指针。

### 扩展解题方法：

1. **增强链表功能**：
   - 除了反转，可以实现更多链表操作，如插入、删除节点，或者实现迭代器以支持更高级的迭代功能。
   - 对链表实现泛型，支持存储各种类型的数据。

2. **优化内存使用**：
   - 考虑使用更高效的内存分配策略，如使用定制的内存分配器。
   - 在不需要双向遍历的场景中使用单链表以减少内存使用。

3. **安全性改进**：
   - 在不改变 API 的前提下，封装裸指针的使用，提供安全的接口。
   - 使用智能指针如 `Rc<T>` 或 `Arc<T>` 来管理节点的生命周期，尤其是在链表被多个所有者共享时。

4. **测试和验证**：
   - 编写更全面的测试用例，覆盖所有新添加的方法和边缘情况。
   - 使用 Rust 的模块化测试特性来组织测试代码，确保每个功能模块都能独立测试。

通过对这些扩展点的实现，可以增强链表的功能和性能，同时保持代码的可维护性和安全性。
