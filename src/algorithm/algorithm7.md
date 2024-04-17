# Exercise 107

- Name: ```algorithm7```
- Path: ```exercises/algorithm/algorithm7.rs```
#### Hint: 

No hints this time!


---



```rust,editable
/*
	stack
	This question requires you to use a stack to achieve a bracket match
*/

// I AM NOT DONE
#[derive(Debug)]
struct Stack<T> {
	size: usize,
	data: Vec<T>,
}
impl<T> Stack<T> {
	fn new() -> Self {
		Self {
			size: 0,
			data: Vec::new(),
		}
	}
	fn is_empty(&self) -> bool {
		0 == self.size
	}
	fn len(&self) -> usize {
		self.size
	}
	fn clear(&mut self) {
		self.size = 0;
		self.data.clear();
	}
	fn push(&mut self, val: T) {
		self.data.push(val);
		self.size += 1;
	}
	fn pop(&mut self) -> Option<T> {
		// TODO
		None
	}
	fn peek(&self) -> Option<&T> {
		if 0 == self.size {
			return None;
		}
		self.data.get(self.size - 1)
	}
	fn peek_mut(&mut self) -> Option<&mut T> {
		if 0 == self.size {
			return None;
		}
		self.data.get_mut(self.size - 1)
	}
	fn into_iter(self) -> IntoIter<T> {
		IntoIter(self)
	}
	fn iter(&self) -> Iter<T> {
		let mut iterator = Iter { 
			stack: Vec::new() 
		};
		for item in self.data.iter() {
			iterator.stack.push(item);
		}
		iterator
	}
	fn iter_mut(&mut self) -> IterMut<T> {
		let mut iterator = IterMut { 
			stack: Vec::new() 
		};
		for item in self.data.iter_mut() {
			iterator.stack.push(item);
		}
		iterator
	}
}
struct IntoIter<T>(Stack<T>);
impl<T: Clone> Iterator for IntoIter<T> {
	type Item = T;
	fn next(&mut self) -> Option<Self::Item> {
		if !self.0.is_empty() {
			self.0.size -= 1;self.0.data.pop()
		} 
		else {
			None
		}
	}
}
struct Iter<'a, T: 'a> {
	stack: Vec<&'a T>,
}
impl<'a, T> Iterator for Iter<'a, T> {
	type Item = &'a T;
	fn next(&mut self) -> Option<Self::Item> {
		self.stack.pop()
	}
}
struct IterMut<'a, T: 'a> {
	stack: Vec<&'a mut T>,
}
impl<'a, T> Iterator for IterMut<'a, T> {
	type Item = &'a mut T;
	fn next(&mut self) -> Option<Self::Item> {
		self.stack.pop()
	}
}

fn bracket_match(bracket: &str) -> bool
{
	//TODO
	true
}

#[cfg(test)]
mod tests {
	use super::*;
	
	#[test]
	fn bracket_matching_1(){
		let s = "(2+3){func}[abc]";
		assert_eq!(bracket_match(s),true);
	}
	#[test]
	fn bracket_matching_2(){
		let s = "(2+3)*(3-1";
		assert_eq!(bracket_match(s),false);
	}
	#[test]
	fn bracket_matching_3(){
		let s = "{{([])}}";
		assert_eq!(bracket_match(s),true);
	}
	#[test]
	fn bracket_matching_4(){
		let s = "{{(}[)]}";
		assert_eq!(bracket_match(s),false);
	}
	#[test]
	fn bracket_matching_5(){
		let s = "[[[]]]]]]]]]";
		assert_eq!(bracket_match(s),false);
	}
	#[test]
	fn bracket_matching_6(){
		let s = "";
		assert_eq!(bracket_match(s),true);
	}
}
```

---

### 本题内容

本题的目标是通过实现一个基本的栈结构来解决括号匹配问题。括号匹配是编程中常见的问题，通常用于验证代码中的括号（如 `{}`, `[]`, `()`）是否正确配对和嵌套。

### 相关知识点：

1. **栈（Stack）的概念**：
   - 栈是一种先进后出（LIFO）的数据结构，只允许在栈顶进行添加元素或移除元素的操作。
   - 在这种结构中，最后进入的元素将是第一个被移除的。

2. **括号匹配逻辑**：
   - 当遇到开放括号时（如 `(`, `{`, `[`），将其压入栈中。
   - 当遇到闭合括号时（如 `)`, `}`, `]`），检查栈顶元素：
     - 如果栈顶元素是匹配的开放括号，就从栈中弹出。
     - 如果不匹配或栈为空，则表明括号无法正确匹配。

3. **Rust 中的 `Vec` 作为栈的实现**：
   - `Vec` 是 Rust 中的动态数组，可以很方便地用作栈。
   - 使用 `Vec::push` 来压栈，使用 `Vec::pop` 来出栈。

### 解题方法：

1. **实现栈的基本操作**：
   - 包括 `push`、`pop`、`is_empty`、`peek` 等。
   
2. **实现括号匹配函数**：
   - 使用栈来跟踪未匹配的开放括号。
   - 遍历给定的字符串，对每个字符进行检查和相应的栈操作。
   - 最后检查栈是否为空，空栈表示所有括号都已正确匹配。

### 代码示例：

这里是实现 `pop` 方法和 `bracket_match` 函数的示例代码：

```rust
impl<T> Stack<T> {
    fn pop(&mut self) -> Option<T> {
        if !self.is_empty() {
            self.size -= 1;
            self.data.pop()
        } else {
            None
        }
    }
}

fn bracket_match(bracket: &str) -> bool {
    let mut stack = Stack::new();
    let brackets = bracket.chars();

    for ch in brackets {
        match ch {
            '(' | '{' | '[' => stack.push(ch),
            ')' => if stack.pop() != Some('(') { return false; },
            '}' => if stack.pop() != Some('{') { return false; },
            ']' => if stack.pop() != Some('[') { return false; },
            _ => (),
        }
    }

    stack.is_empty()
}
```

这段代码将 `pop` 方法完善，并在 `bracket_match` 函数中实现了使用栈进行括号匹配的逻辑。通过遍历输入的字符串，并针对不同类型的字符执行相应的栈操作，我们可以验证括号是否正确匹配。

## 扩展知识点与解答：

### 扩展知识点

1. **栈的进一步应用**:
   - 栈不仅可以用于括号匹配，还广泛应用于表达式求值（如后缀表达式计算）、函数调用的内存管理（调用栈）、历史记录功能（如浏览器后退）等。
   - 栈的结构使得最后进行的操作可以首先被撤销或再次访问，这在很多需要这种访问模式的算法中非常有用。

2. **括号匹配的变体**:
   - 括号匹配不仅限于简单的几种括号，它可以扩展到包含各种类型的括号，包括但不限于角括号`<`和`>`，甚至自定义配对符号。
   - 在某些编程语言或文本格式中，括号的匹配也可能涉及到转义字符，比如在字符串中的括号可能不需要匹配。

3. **复杂性分析**:
   - 栈操作（push 和 pop）的时间复杂度为 O(1)。
   - 对于长度为 n 的输入字符串，括号匹配的总体时间复杂度为 O(n)，因为每个字符最多被处理两次（一次压栈，一次弹栈）。

### 扩展解题方法

1. **优化括号匹配**:
   - 可以通过建立一个映射来简化括号的匹配逻辑，例如使用一个哈希表来存储每种关闭括号与其对应的开放括号。
   - 这样，在处理关闭括号时，可以直接查找哈希表来验证栈顶元素是否匹配，而无需多个 if 判断。

2. **错误提示**:
   - 在括号匹配中添加更多的错误处理逻辑，例如记录错误位置和错误类型（比如是缺失关闭括号还是括号类型不匹配），可以帮助开发者更快地定位和解决代码中的问题。

3. **可视化工具**:
   - 开发一个工具，它不仅可以检查文本中的括号是否匹配，还可以可视化地显示括号的匹配过程。这对于教学或在复杂文本中调试括号错误特别有用。

通过上述扩展知识点和解题方法，可以进一步深化对栈结构的理解，并在实际编程和算法设计中有效利用这一数据结构。
