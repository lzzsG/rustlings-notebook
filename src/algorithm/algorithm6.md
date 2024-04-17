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

在这个练习中，你需要实现一个基本的深度优先搜索（DFS）算法来遍历图。DFS 是一种用于遍历或搜索树或图的算法。DFS 从一个起始点开始，沿当前顶点的边进行遍历，如果有未访问的节点则递归地访问它，直到所有的顶点都被探访过。它使用一个栈来实现递归功能。

### 目标
本练习的目标是理解并实现 DFS 算法，以便能够遍历给定的图，同时记录访问顶点的顺序。

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
