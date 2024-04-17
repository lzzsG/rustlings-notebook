# Exercise 106

- Name: ```algorithm6```
- Path: ```exercises/algorithm/algorithm6.rs```
#### Hint: 

No hints this time!


---



```rust,editable
/*
	dfs
	This problem requires you to implement a basic DFS traversal
*/

// I AM NOT DONE
use std::collections::HashSet;

struct Graph {
    adj: Vec<Vec<usize>>, 
}

impl Graph {
    fn new(n: usize) -> Self {
        Graph {
            adj: vec![vec![]; n],
        }
    }

    fn add_edge(&mut self, src: usize, dest: usize) {
        self.adj[src].push(dest);
        self.adj[dest].push(src); 
    }

    fn dfs_util(&self, v: usize, visited: &mut HashSet<usize>, visit_order: &mut Vec<usize>) {
        //TODO
    }

    // Perform a depth-first search on the graph, return the order of visited nodes
    fn dfs(&self, start: usize) -> Vec<usize> {
        let mut visited = HashSet::new();
        let mut visit_order = Vec::new(); 
        self.dfs_util(start, &mut visited, &mut visit_order);
        visit_order
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_dfs_simple() {
        let mut graph = Graph::new(3);
        graph.add_edge(0, 1);
        graph.add_edge(1, 2);

        let visit_order = graph.dfs(0);
        assert_eq!(visit_order, vec![0, 1, 2]);
    }

    #[test]
    fn test_dfs_with_cycle() {
        let mut graph = Graph::new(4);
        graph.add_edge(0, 1);
        graph.add_edge(0, 2);
        graph.add_edge(1, 2);
        graph.add_edge(2, 3);
        graph.add_edge(3, 3); 

        let visit_order = graph.dfs(0);
        assert_eq!(visit_order, vec![0, 1, 2, 3]);
    }

    #[test]
    fn test_dfs_disconnected_graph() {
        let mut graph = Graph::new(5);
        graph.add_edge(0, 1);
        graph.add_edge(0, 2);
        graph.add_edge(3, 4); 

        let visit_order = graph.dfs(0);
        assert_eq!(visit_order, vec![0, 1, 2]); 
        let visit_order_disconnected = graph.dfs(3);
        assert_eq!(visit_order_disconnected, vec![3, 4]); 
    }
}
```

---

### 本题内容

在这个练习中，你需要实现一个基本的深度优先搜索（DFS）算法来遍历图。DFS 是一种用于遍历或搜索树或图的算法。DFS 从一个起始点开始，沿当前顶点的边进行遍历，如果有未访问的节点则递归地访问它，直到所有的顶点都被探访过。它使用一个栈来实现递归功能。

### 相关知识点
1. **图的表示**：使用邻接表来表示图，其中每个节点指向一个包含其所有邻居的列表。
2. **递归搜索**：DFS 使用递归来实现深入搜索，直到所有可能的路径都被探索完毕。
3. **访问控制**：使用 `HashSet` 来记录已访问的节点，防止节点被重复访问，这对于包含环的图尤其重要。

### 解题方法
1. **初始化**：首先，初始化一个图实例，并根据需要添加边。
2. **DFS 功能实现**：
   - 使用一个递归函数 `dfs_util` 来进行深度优先搜索。这个函数检查当前节点是否已被访问，如果没有，则标记为已访问，并对所有邻接的未访问节点递归调用自身。
   - 将访问的节点添加到 `visit_order` 列表中，这个列表将记录访问顶点的顺序。
3. **边界检查**：确保代码可以处理不同的图形状况，包括孤立的节点和环。

### 代码示例
以下是 `dfs_util` 函数的示例实现：

```rust
fn dfs_util(&self, v: usize, visited: &mut HashSet<usize>, visit_order: &mut Vec<usize>) {
    // 标记当前节点为已访问
    visited.insert(v);
    // 将节点添加到访问顺序列表
    visit_order.push(v);

    // 遍历当前节点的所有邻接节点
    for &adj_node in &self.adj[v] {
        if !visited.contains(&adj_node) {
            self.dfs_util(adj_node, visited, visit_order);
        }
    }
}
```

这个函数体现了 DFS 的核心，即递归地访问每个未被访问过的邻接节点，并在访问过程中记录顺序。通过这种方式，你可以实现对图的完整遍历，并能够返回从任意起点开始的节点访问顺序。

## 扩展知识点与解答：

### 扩展知识点

1. **递归与非递归DFS**:
   - **递归DFS**是最直观的实现方式，如上面所示。它利用递归调用栈来回溯。
   - **非递归DFS**使用一个显式的栈数据结构来模拟递归过程。这种方法在处理非常大的图时，可以避免深度递归可能导致的栈溢出问题。

2. **图的遍历顺序**:
   - DFS的特点是尽可能深地遍历图的分支，这种特性使其在某些图算法中非常有用，如寻找连通分量、拓扑排序等。
   - 在有向图中，DFS可以用来检测环。

3. **图的连通性**:
   - 在调用DFS之后，可以检查 `visited` 集合的大小来确定是否所有的顶点都被访问过，从而检测图的连通性。
   - 对于非连通图，可以对每个未访问的顶点运行DFS，以探索图中的所有连通分量。

4. **时间复杂度**:
   - DFS的时间复杂度为 \(O(V + E)\)，其中 \(V\) 是顶点数，\(E\) 是边数。这是因为每个顶点和每条边最多被访问一次。

5. **空间复杂度**:
   - 在递归实现中，空间复杂度主要由递归调用栈的最大深度决定，这在最坏情况下可能达到 \(O(V)\)。
   - 在非递归实现中，空间复杂度由栈的大小决定，也是 \(O(V)\)。

### 扩展解题方法

1. **非递归DFS实现**:
   ```rust
   fn dfs_non_recursive(&self, start: usize) -> Vec<usize> {
       let mut stack = Vec::new();
       let mut visited = HashSet::new();
       let mut visit_order = Vec::new();
   
       stack.push(start);
   
       while let Some(v) = stack.pop() {
           if visited.insert(v) {
               visit_order.push(v);
               for &adj_node in self.adj[v].iter().rev() {
                   if !visited.contains(&adj_node) {
                       stack.push(adj_node);
                   }
               }
           }
       }
   
       visit_order
   }
   ```

2. **应用DFS进行拓扑排序**:
   - 拓扑排序是有向无环图（DAG）的线性排序，其中对于每一条有向边 \(U \rightarrow V\)，顶点 \(U\) 都在顶点 \(V\) 之前。
   - 可以通过DFS实现，具体方法是在DFS访问的过程中，将每个顶点在递归结束时加入到一个前端列表中。

3. **使用DFS检测有向图中的环**:
   - 在DFS过程中，如果在访问一个节点的所有邻接节点时发现某个邻接节点已经在当前递归栈中，则图中存在环。

这些扩展知识点和解题方法能帮助你更深入地理解DFS的工作原理和应用场景，从而在实际问题中灵活运用这一算法。

## `HashSet` 

`HashSet` 在 Rust 中是一种基于哈希表的集合，它存储唯一的元素并提供快速的查找、插入和删除操作。在许多算法中，特别是涉及到元素去重或快速查找元素是否存在的场景中，`HashSet` 都是一个非常有用的数据结构。

### 核心特性

1. **唯一性**: `HashSet` 保证其中的每个元素都是唯一的，当你尝试添加一个已存在的元素时，操作会失败或不做任何改变（取决于使用的方法）。

2. **性能**: `HashSet` 提供平均时间复杂度为 \(O(1)\) 的插入、移除和查找操作。实际性能会受到哈希函数质量的影响。

3. **哈希函数**: `HashSet` 依赖于元素的 `Hash` 和 `Eq` trait 实现。自定义类型在使用 `HashSet` 前需要确保已经适当实现这两个 trait。

### 常用方法

- **插入**: `insert(&mut self, value: T) -> bool`，尝试将值插入集合中。如果值已存在，返回 `false`，否则插入并返回 `true`。
  
- **删除**: `remove(&mut self, value: &T) -> bool`，如果集合中存在该值，则删除并返回 `true`；如果不存在，返回 `false`。
  
- **包含**: `contains(&self, value: &T) -> bool`，检查集合中是否包含某个值。
  
- **大小**: `len(&self) -> usize`，返回集合中元素的数量。
  
- **清空**: `clear(&mut self)`，移除集合中的所有元素。

### 示例：用户访问去重

假设我们正在开发一个网站，需要追踪用户的访问记录，确保同一用户在同一天内的多次访问只被记录一次。这里，我们使用 `HashSet` 来存储每日的用户访问记录。

```rust
use std::collections::HashSet;

#[derive(Hash, Eq, PartialEq, Debug)]
struct UserVisit {
    user_id: String,
    visit_date: String, // 示例中使用字符串格式日期
}

fn main() {
    let mut visits = HashSet::new();

    // 假设这些是从数据库或请求中获取的数据
    let visit1 = UserVisit {
        user_id: "user123".to_string(),
        visit_date: "2021-09-15".to_string(),
    };

    let visit2 = UserVisit {
        user_id: "user124".to_string(),
        visit_date: "2021-09-15".to_string(),
    };

    let visit3 = UserVisit {
        user_id: "user123".to_string(),
        visit_date: "2021-09-15".to_string(),
    };

    // 尝试添加访问记录
    println!("Adding visit1: {}", visits.insert(visit1)); // 应该成功，输出 true
    println!("Adding visit2: {}", visits.insert(visit2)); // 应该成功，输出 true
    println!("Adding visit3: {}", visits.insert(visit3)); // 应该失败，输出 false 因为 user123 在同一天的访问已存在

    // 显示所有访问记录
    println!("Visit records:");
    for visit in &visits {
        println!("{:?}", visit);
    }
}
```

在这个示例中，我们定义了一个 `UserVisit` 结构体，它包含用户 ID 和访问日期。为了能在 `HashSet` 中使用，我们需要为这个结构体实现 `Hash`、`Eq` 和 `PartialEq` trait。Rust 允许我们通过派生宏来自动实现这些 trait。

`HashSet` 的 `insert` 方法尝试将元素插入集合中。如果元素已存在（即集合中已有相同的 `user_id` 和 `visit_date`），则返回 `false` 并且不会添加元素。这就自然地实现了去重功能。

这个示例清楚地展示了 `HashSet` 如何帮助我们快速管理不需要重复记录的数据，同时提供了非常高效的数据查找和插入性能。

`HashSet` 是处理涉及快速成员访问和保证元素唯一性的问题的理想选择。在图算法、去重任务、以及需要快速查找操作的任何场景中，都可以考虑使用 `HashSet`。
