# Exercise 108

- Name: ```algorithm8```
- Path: ```exercises/algorithm/algorithm8.rs```
#### Hint: 

No hints this time!


---



```rust,editable
/*
	queue
	This question requires you to use queues to implement the functionality of the stac
*/
// I AM NOT DONE

#[derive(Debug)]
pub struct Queue<T> {
    elements: Vec<T>,
}

impl<T> Queue<T> {
    pub fn new() -> Queue<T> {
        Queue {
            elements: Vec::new(),
        }
    }

    pub fn enqueue(&mut self, value: T) {
        self.elements.push(value)
    }

    pub fn dequeue(&mut self) -> Result<T, &str> {
        if !self.elements.is_empty() {
            Ok(self.elements.remove(0usize))
        } else {
            Err("Queue is empty")
        }
    }

    pub fn peek(&self) -> Result<&T, &str> {
        match self.elements.first() {
            Some(value) => Ok(value),
            None => Err("Queue is empty"),
        }
    }

    pub fn size(&self) -> usize {
        self.elements.len()
    }

    pub fn is_empty(&self) -> bool {
        self.elements.is_empty()
    }
}

impl<T> Default for Queue<T> {
    fn default() -> Queue<T> {
        Queue {
            elements: Vec::new(),
        }
    }
}

pub struct myStack<T>
{
	//TODO
	q1:Queue<T>,
	q2:Queue<T>
}
impl<T> myStack<T> {
    pub fn new() -> Self {
        Self {
			//TODO
			q1:Queue::<T>::new(),
			q2:Queue::<T>::new()
        }
    }
    pub fn push(&mut self, elem: T) {
        //TODO
    }
    pub fn pop(&mut self) -> Result<T, &str> {
        //TODO
		Err("Stack is empty")
    }
    pub fn is_empty(&self) -> bool {
		//TODO
        true
    }
}

#[cfg(test)]
mod tests {
	use super::*;
	
	#[test]
	fn test_queue(){
		let mut s = myStack::<i32>::new();
		assert_eq!(s.pop(), Err("Stack is empty"));
        s.push(1);
        s.push(2);
        s.push(3);
        assert_eq!(s.pop(), Ok(3));
        assert_eq!(s.pop(), Ok(2));
        s.push(4);
        s.push(5);
        assert_eq!(s.is_empty(), false);
        assert_eq!(s.pop(), Ok(5));
        assert_eq!(s.pop(), Ok(4));
        assert_eq!(s.pop(), Ok(1));
        assert_eq!(s.pop(), Err("Stack is empty"));
        assert_eq!(s.is_empty(), true);
	}
}
```

---

### 本题内容

本练习的目标是让学生实现一个使用两个队列（queue）模拟栈（stack）的功能。这种数据结构转换是一个常见的编程练习，用于深化对队列和栈的理解，并展示如何使用简单的数据结构来模拟复杂的行为。

### 相关知识点

1. **队列 (Queue)**:
   - 队列是一种先进先出（FIFO）的数据结构。
   - 在 Rust 中，队列通常可以通过 `Vec` 或 `VecDeque` 来实现。

2. **栈 (Stack)**:
   - 栈是一种后进先出（LIFO）的数据结构。
   - 在这种结构中，最后被加入的元素将是第一个被移除的。

3. **队列模拟栈**:
   - 使用两个队列交替保存数据，可以模拟栈的操作。
   - 一个队列用于存放新入栈的元素，当需要执行出栈操作时，将这个队列的元素依次出队并入队到另一个队列，直到最后一个元素，这最后一个元素即为栈顶元素，应被弹出。

### 解题方法

1. **初始化**:
   - 创建两个队列 `q1` 和 `q2` 来存储栈元素。

2. **Push 操作**:
   - 将元素加入到非空的队列（如果两个队列都空，则选择任意一个）。

3. **Pop 操作**:
   - 将非空队列中的所有元素（除最后一个元素外）移动到另一个空队列，然后移除并返回最后一个元素。
   - 这个操作模拟了栈的 LIFO 特性。

4. **isEmpty 操作**:
   - 如果两个队列都为空，则栈为空。

### 代码示例

这里提供了一个基本框架来实现 `push`、`pop` 和 `is_empty` 操作。具体的实现需要根据队列的基本操作（如 `enqueue` 和 `dequeue`）来完成。

```rust
impl<T> myStack<T> {
    pub fn push(&mut self, elem: T) {
        // 将元素总是加入到 q1
        self.q1.enqueue(elem);
    }

    pub fn pop(&mut self) -> Result<T, &str> {
        if self.is_empty() {
            return Err("Stack is empty");
        }

        // 将 q1 中的元素移至 q2，直到 q1 中只剩一个元素
        while self.q1.size() > 1 {
            if let Some(val) = self.q1.dequeue().ok() {
                self.q2.enqueue(val);
            }
        }

        // q1 的最后一个元素即为栈顶元素
        let result = self.q1.dequeue();

        // 交换 q1 和 q2
        std::mem::swap(&mut self.q1, &mut self.q2);

        result
    }

    pub fn is_empty(&self) -> bool {
        self.q1.is_empty() && self.q2.is_empty()
    }
}
```

这个实现利用了两个队列来模拟一个栈的行为，每次 `pop` 操作都通过移动元素来确保最后加入的元素能够第一个出来，从而满足栈的后进先出特性。

## 扩展知识点与解答：

### 扩展知识点

1. **数据结构的互相模拟**:
   - 使用一种数据结构来模拟另一种常见数据结构是计算机科学中的一个有趣的主题。这不仅帮助理解每种结构的内部工作原理，也是一种学习如何更有效利用已有工具的好方法。
   - 在本题中，通过两个队列模拟栈的操作，你可以更深入地理解队列和栈的基本操作及其性能影响。

2. **时间复杂度分析**:
   - 了解不同操作的时间复杂度对于优化算法和选择正确的数据结构非常重要。
   - 在使用两个队列模拟栈的情况下，`push` 操作的时间复杂度为 \(O(1)\)，而 `pop` 操作因为涉及到元素的移动，其时间复杂度为 \(O(n)\)，其中 \(n\) 是栈中的元素数量。

3. **Rust 语言特性应用**:
   - Rust 的所有权和借用规则可以有效防止数据竞争和内存泄漏，这在实现如队列这样的数据结构时尤其有用。
   - 利用 Rust 的 `Vec` 实现队列提供了自动内存管理和动态扩容的功能，简化了数据结构的实现。

### 扩展解题方法

1. **另一种实现思路**:
   - 使用一个队列和递归：你可以使用单个队列来模拟栈，每次 `pop` 操作时，通过递归将队列的前面元素移动到队尾，直到只剩下一个元素，然后返回这个元素。

2. **优化策略**:
   - 考虑实现一个 `AmortizedQueue`，该队列在大多数情况下保持 \(O(1)\) 的复杂度，偶尔进行一次完整的内部重组来保证性能。
   - 利用 Rust 的 `std::collections::VecDeque` 来代替 `Vec`，因为 `VecDeque` 在两端都提供了 \(O(1)\) 的 `push` 和 `pop` 操作，更适合本题的需求。

3. **扩展应用**:
   - 考虑实现一种支持多种操作的 "双端队列栈"，可以从两端进行 `push` 和 `pop` 操作。
   - 这种数据结构可以用来解决更复杂的算法问题，如滑动窗口最大值等。

通过这样的扩展学习，你不仅能够深入理解栈和队列的基本操作，还能掌握它们在实际编程和算法问题中的应用。

## 代码解释

在这个问题中，我们需要使用两个队列 (`Queue`) 实现一个栈 (`myStack`) 的基本功能。下面将详细解释每个部分的实现和工作原理。

### 队列 (`Queue`) 实现
首先，让我们来看看 `Queue` 的实现，这是实现栈的基础。

#### 构造函数
构造函数简单地初始化一个空的 `Vec<T>` 来存储队列元素：
```rust
pub fn new() -> Queue<T> {
    Queue {
        elements: Vec::new(),
    }
}
```

#### 入队 (`enqueue`)
`enqueue` 方法将一个元素添加到队列的尾部：
```rust
pub fn enqueue(&mut self, value: T) {
    self.elements.push(value)
}
```
这里使用 `Vec::push` 方法，因为在队列中，新元素总是添加到队尾。

#### 出队 (`dequeue`)
`dequeue` 方法从队列的头部移除元素，并返回它。如果队列为空，则返回一个错误：
```rust
pub fn dequeue(&mut self) -> Result<T, &str> {
    if !self.elements.is_empty() {
        Ok(self.elements.remove(0))
    } else {
        Err("Queue is empty")
    }
}
```
这里使用 `Vec::remove` 方法从向量的开始位置（索引0）移除元素，这对应于队列的第一个元素。

#### 查看队首元素 (`peek`)
`peek` 方法返回队列第一个元素的引用，不移除它：
```rust
pub fn peek(&self) -> Result<&T, &str> {
    match self.elements.first() {
        Some(value) => Ok(value),
        None => Err("Queue is empty"),
    }
}
```
这里使用 `Vec::first` 方法，它返回一个 `Option`，表明是否存在元素。

### 栈 (`myStack`) 使用两个队列实现
接下来，我们看看如何使用两个队列 `q1` 和 `q2` 来模拟栈的操作。

#### 构造函数
在 `myStack` 的构造函数中，我们初始化两个空队列：
```rust
pub fn new() -> Self {
    Self {
        q1: Queue::<T>::new(),
        q2: Queue::<T>::new()
    }
}
```

#### 入栈 (`push`)
入栈操作简单地将元素加入到 `q1`：
```rust
pub fn push(&mut self, elem: T) {
    self.q1.enqueue(elem);
}
```

#### 出栈 (`pop`)
出栈操作稍微复杂一些，我们需要保证最后加入 `q1` 的元素第一个被移除（后进先出）。为此，我们将 `q1` 的所有元素（除了最后一个）转移到 `q2`，然后移除并返回 `q1` 的最后一个元素。之后，我们交换 `q1` 和 `q2` 的角色，以便下一次操作：
```rust
pub fn pop(&mut self) -> Result<T, &str> {
    if self.is_empty() {
        return Err("Stack is empty");
    }

    while self.q1.size() > 1 {
        if let Some(val) = self.q1.dequeue().ok() {
            self.q2.enqueue(val);
        }
    }

    let result = self.q1.dequeue();
    std::mem::swap(&mut self.q1, &mut self.q2);

    result
}
```

#### 栈是否为空 (`is_empty`)
检查栈是否为空即检查两个队列是否都为空：
```rust
pub fn is_empty(&self) -> bool {
    self.q1.is_empty() && self.q2.is_empty()
}
```

通过这种方式，我们使用两个队列成功地模拟了一个栈的行为。在这个实现中，每个元素最多被移动两次（从 `q1` 到 `q2`，然后在交换后可能再次从新的 `q1`（原来的 `q2`）移动到新的 `q2`（原来的 `q1`））。
