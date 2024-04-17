# Exercise 105

- Name: ```algorithm5```
- Path: ```exercises/algorithm/algorithm5.rs```
#### Hint: 

No hints this time!


---



```rust,editable
/*
	bfs
	This problem requires you to implement a basic BFS algorithm
*/

//I AM NOT DONE
use std::collections::VecDeque;

// Define a graph
struct Graph {
    adj: Vec<Vec<usize>>, 
}

impl Graph {
    // Create a new graph with n vertices
    fn new(n: usize) -> Self {
        Graph {
            adj: vec![vec![]; n],
        }
    }

    // Add an edge to the graph
    fn add_edge(&mut self, src: usize, dest: usize) {
        self.adj[src].push(dest); 
        self.adj[dest].push(src); 
    }

    // Perform a breadth-first search on the graph, return the order of visited nodes
    fn bfs_with_return(&self, start: usize) -> Vec<usize> {
        
		//TODO

        let mut visit_order = vec![];
        visit_order
    }
}


#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_bfs_all_nodes_visited() {
        let mut graph = Graph::new(5);
        graph.add_edge(0, 1);
        graph.add_edge(0, 4);
        graph.add_edge(1, 2);
        graph.add_edge(1, 3);
        graph.add_edge(1, 4);
        graph.add_edge(2, 3);
        graph.add_edge(3, 4);

        let visited_order = graph.bfs_with_return(0);
        assert_eq!(visited_order, vec![0, 1, 4, 2, 3]);
    }

    #[test]
    fn test_bfs_different_start() {
        let mut graph = Graph::new(3);
        graph.add_edge(0, 1);
        graph.add_edge(1, 2);

        let visited_order = graph.bfs_with_return(2);
        assert_eq!(visited_order, vec![2, 1, 0]);
    }

    #[test]
    fn test_bfs_with_cycle() {
        let mut graph = Graph::new(3);
        graph.add_edge(0, 1);
        graph.add_edge(1, 2);
        graph.add_edge(2, 0);

        let visited_order = graph.bfs_with_return(0);
        assert_eq!(visited_order, vec![0, 1, 2]);
    }

    #[test]
    fn test_bfs_single_node() {
        let mut graph = Graph::new(1);

        let visited_order = graph.bfs_with_return(0);
        assert_eq!(visited_order, vec![0]);
    }
}
```

---

### 本题内容

本练习要求实现一个广度优先搜索（BFS）算法来遍历图。BFS 是一种用于图的遍历或搜索的算法，它从根节点开始，逐层访问邻近节点。这种方法使用队列来跟踪接下来应访问的节点，确保每个节点都按照它们第一次被发现的顺序被处理。

### 相关知识点

1. **广度优先搜索 (BFS)**：
   - BFS 通常使用队列来实现，这样可以保证以非递减的距离顺序访问节点。

2. **队列**：
   - BFS 实现中，队列用于存储待访问的节点。Rust 中可以使用 `VecDeque`，它提供了高效的队首和队尾的插入和删除操作。

3. **图的表示**：
   - 在本题中，图使用邻接表来表示，这是一种常见的表示方法，特别适用于边数远小于顶点对数的图。

4. **访问控制**：
   - 使用标记（如数组或 `HashSet`）来记录哪些节点已被访问，防止节点被重复访问，特别是在含有环的图中。

### 解题方法

1. **初始化**：
   - 创建一个 `VecDeque` 用作队列，用于存储将要访问的节点。
   - 初始化一个访问标记数组或集合，用于标记已访问的节点。

2. **遍历每个节点**：
   - 将起始节点加入队列，并标记为已访问。
   - 循环直到队列为空：
     - 从队列前端取出一个节点（当前节点）。
     - 访问该节点（可以是打印、计数等操作）。
     - 将当前节点的所有未访问的邻接节点加入队列，并标记为已访问。

3. **结束**：
   - 当队列为空时，所有可达的节点都已被访问，算法结束。

### 代码示例

实现 `bfs_with_return` 方法，以返回遍历顺序：

```rust
impl<T> Graph<T>
where
    T: Ord + Copy,
{
    pub fn bfs_with_return(&self, start: usize) -> Vec<usize> {
        let mut queue = VecDeque::new();
        let mut visited = vec![false; self.adj.len()];
        let mut visit_order = Vec::new();

        // Start from the given node
        queue.push_back(start);
        visited[start] = true;

        while let Some(node) = queue.pop_front() {
            visit_order.push(node);
            for &adj_node in &self.adj[node] {
                if !visited[adj_node] {
                    visited[adj_node] = true;
                    queue.push_back(adj_node);
                }
            }
        }
        visit_order
    }
}
```

这段代码实现了基本的 BFS 遍历，通过队列管理待访问的节点，并通过数组跟踪已访问节点。通过测试可以验证算法的正确性，包括能处理不同类型的图结构，如有环图、不同起始节点的图等。

## 扩展知识点与解答：

### 扩展知识点

1. **图的表示方法**：
   - 邻接矩阵：使用二维数组表示节点间的连接关系，适用于稠密图。
   - 邻接表：使用链表、向量等数据结构表示节点的邻接关系，适用于稀疏图。

2. **图的遍历算法**：
   - 深度优先搜索（DFS）：另一种图的遍历算法，与 BFS 相比，DFS 会一直向下访问到最深的节点，直到没有未访问的邻居节点为止，然后回溯到上一个节点继续。
   - 最短路径算法：例如 Dijkstra 算法、Bellman-Ford 算法、Floyd-Warshall 算法等，用于找出图中两个节点之间的最短路径。

3. **性能优化**：
   - 对于大型图，可以考虑并行化 BFS 算法以加快遍历速度。
   - 使用哈希集合或位图等数据结构来记录节点的访问状态，以节省内存和提高访问速度。

4. **图的应用**：
   - 网络路由：在计算机网络中，路由器使用图算法来选择最佳路径转发数据包。
   - 社交网络分析：分析社交网络的连接模式和影响力传播。
   - 最小生成树：用于在网络中选择最小成本的连接方式。

### 扩展解题方法

1. **使用并行化算法**：
   - 考虑使用并行化技术（如 Rayon 库）来加速 BFS 算法的执行，特别是对于大型图而言，这样可以充分利用多核处理器的性能。

2. **优化数据结构**：
   - 考虑优化图的表示方式，根据具体情况选择邻接矩阵或邻接表，以提高算法的效率和空间利用率。

3. **应用图算法**：
   - 在解决实际问题时，了解不同的图算法，并根据问题的特点选择合适的算法。例如，在网络路由中选择最短路径算法，而在社交网络分析中可能更关注图的连通性和节点度。

4. **处理大型图**：
   - 对于超大规模的图，可能需要分布式算法或外部存储来处理，考虑使用图数据库等专门的工具和技术。

5. **算法可视化**：
   - 使用可视化工具将图形化展示算法的执行过程，有助于理解和调试算法，例如使用 GraphViz、Gephi 等工具。

## `VecDeque` 

`VecDeque` 是 Rust 标准库中提供的一个双端队列（double-ended queue）的数据结构。它在 `std::collections` 模块中定义，提供了高效的元素插入和删除操作。`VecDeque` 允许你在队列的前端和后端添加或移除元素，这使得它非常适合实现如广度优先搜索（BFS）这样的算法，其中需要频繁地从队列的前端移除元素，并从后端添加元素。

### 主要特性

1. **动态扩展**：`VecDeque` 在内部使用一个动态数组来存储数据，当数组满时，它会自动扩展容量。

2. **双端操作**：
   - `push_front` 和 `push_back` 方法允许在 `VecDeque` 的前端或后端插入元素。
   - `pop_front` 和 `pop_back` 方法则允许从前端或后端移除元素，并返回被移除的元素。

3. **随机访问**：通过索引直接访问元素，执行时间为 O(1)。

4. **内存效率**：`VecDeque` 使用循环缓冲区技术，以最大限度地利用分配的内存空间。

### 应用场景

`VecDeque` 是实现队列和双端队列结构的理想选择，特别适用于需要双端插入或删除操作的场景，例如：
- 广度优先搜索（BFS）中，需要一个队列来存储待访问的节点。
- 实现一个滑动窗口算法，其中可能需要从两端添加或移除元素。
- 编写解析器或其他需要从两端动态添加或删除数据的算法。

### 代码示例：回文检测器

这里是一个使用 `VecDeque` 来解决问题的不同示例，我们将使用它来实现一个简单的回文检测器。回文是正读和反读都相同的字符串（例如 "radar" 或 "level"）。我们将利用 `VecDeque` 的双端操作功能来比较字符串的前端和后端字符。

```rust
use std::collections::VecDeque;

fn is_palindrome(s: &str) -> bool {
    let mut deque: VecDeque<char> = s.chars().collect();

    while deque.len() > 1 {
        if deque.pop_front() != deque.pop_back() {
            return false;
        }
    }

    true
}

fn main() {
    let strings = ["radar", "apple", "level", "hello"];
    for &s in &strings {
        println!("Is '{}' a palindrome? {}", s, is_palindrome(s));
    }
}
```

#### 如何工作

1. **字符串到双端队列的转换**：首先，我们将输入的字符串转换为一个 `char` 类型的 `VecDeque`。每个字符按顺序存储在双端队列中。

2. **双端比较**：我们循环检查双端队列的前端和后端元素是否相同，这是检测回文的关键。
   - 使用 `pop_front` 从队列的前端移除一个元素。
   - 使用 `pop_back` 从队列的后端移除一个元素。
   - 如果这两个元素不相等，则字符串不是回文，函数返回 `false`。

3. **队列长度判断**：如果队列中的元素少于两个，我们就停止比较，因为已经没有更多的字符可以比较或者剩下的单个字符不影响回文的判断。

4. **返回结果**：如果所有比较都显示字符相同，那么字符串是一个回文，函数返回 `true`。

这个示例展示了如何使用 `VecDeque` 的基本操作来实现一个实际的算法问题解决方案，同时利用其双端操作的优势，提高算法效率。
