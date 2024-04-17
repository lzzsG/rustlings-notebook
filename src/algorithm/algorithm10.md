# Exercise 110

- Name: ```algorithm10```
- Path: ```exercises/algorithm/algorithm10.rs```
#### Hint: 

No hints this time!


---



```rust,editable
/*
	graph
	This problem requires you to implement a basic graph function
*/
// I AM NOT DONE

use std::collections::{HashMap, HashSet};
use std::fmt;
#[derive(Debug, Clone)]
pub struct NodeNotInGraph;
impl fmt::Display for NodeNotInGraph {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "accessing a node that is not in the graph")
    }
}
pub struct UndirectedGraph {
    adjacency_table: HashMap<String, Vec<(String, i32)>>,
}
impl Graph for UndirectedGraph {
    fn new() -> UndirectedGraph {
        UndirectedGraph {
            adjacency_table: HashMap::new(),
        }
    }
    fn adjacency_table_mutable(&mut self) -> &mut HashMap<String, Vec<(String, i32)>> {
        &mut self.adjacency_table
    }
    fn adjacency_table(&self) -> &HashMap<String, Vec<(String, i32)>> {
        &self.adjacency_table
    }
    fn add_edge(&mut self, edge: (&str, &str, i32)) {
        //TODO
    }
}
pub trait Graph {
    fn new() -> Self;
    fn adjacency_table_mutable(&mut self) -> &mut HashMap<String, Vec<(String, i32)>>;
    fn adjacency_table(&self) -> &HashMap<String, Vec<(String, i32)>>;
    fn add_node(&mut self, node: &str) -> bool {
        //TODO
		true
    }
    fn add_edge(&mut self, edge: (&str, &str, i32)) {
        //TODO
    }
    fn contains(&self, node: &str) -> bool {
        self.adjacency_table().get(node).is_some()
    }
    fn nodes(&self) -> HashSet<&String> {
        self.adjacency_table().keys().collect()
    }
    fn edges(&self) -> Vec<(&String, &String, i32)> {
        let mut edges = Vec::new();
        for (from_node, from_node_neighbours) in self.adjacency_table() {
            for (to_node, weight) in from_node_neighbours {
                edges.push((from_node, to_node, *weight));
            }
        }
        edges
    }
}
#[cfg(test)]
mod test_undirected_graph {
    use super::Graph;
    use super::UndirectedGraph;
    #[test]
    fn test_add_edge() {
        let mut graph = UndirectedGraph::new();
        graph.add_edge(("a", "b", 5));
        graph.add_edge(("b", "c", 10));
        graph.add_edge(("c", "a", 7));
        let expected_edges = [
            (&String::from("a"), &String::from("b"), 5),
            (&String::from("b"), &String::from("a"), 5),
            (&String::from("c"), &String::from("a"), 7),
            (&String::from("a"), &String::from("c"), 7),
            (&String::from("b"), &String::from("c"), 10),
            (&String::from("c"), &String::from("b"), 10),
        ];
        for edge in expected_edges.iter() {
            assert_eq!(graph.edges().contains(edge), true);
        }
    }
}
```

---

### 本题内容

在这个问题中，我们需要实现一个基本的无向图功能，包括节点和边的添加，以及对图结构的查询。以下是具体的实现和解释：

### 图的基础结构
我们使用 `HashMap` 来存储图的邻接表，每个键值对应一个节点和一个向量，该向量包含与该节点相连的其他节点及其边的权重。

```rust
pub struct UndirectedGraph {
    adjacency_table: HashMap<String, Vec<(String, i32)>>,
}
```

### Graph Trait
定义了图的行为，这包括：
- 创建新图
- 添加节点和边
- 检查图中是否包含特定节点
- 获取图中的所有节点
- 获取图中的所有边

```rust
pub trait Graph {
    fn new() -> Self;
    fn adjacency_table_mutable(&mut self) -> &mut HashMap<String, Vec<(String, i32)>>;
    fn adjacency_table(&self) -> &HashMap<String, Vec<(String, i32)>>;
    fn add_node(&mut self, node: &str) -> bool;
    fn add_edge(&mut self, edge: (&str, &str, i32));
    fn contains(&self, node: &str) -> bool;
    fn nodes(&self) -> HashSet<&String>;
    fn edges(&self) -> Vec<(&String, &String, i32)>;
}
```

### 实现方法

#### 新建图
这是创建一个新图的构造函数，初始化空的邻接表。

```rust
fn new() -> Self {
    UndirectedGraph {
        adjacency_table: HashMap::new(),
    }
}
```

#### 添加边
此方法更新邻接表，以包含两个节点之间的新边，由于是无向图，所以需要在两个方向上添加。

```rust
fn add_edge(&mut self, edge: (&str, &str, i32)) {
    let (from, to, weight) = edge;
    self.adjacency_table.entry(from.to_string()).or_default().push((to.to_string(), weight));
    self.adjacency_table.entry(to.to_string()).or_default().push((from.to_string(), weight));
}
```

#### 添加节点
这是尝试将新节点添加到图中的方法。

```rust
fn add_node(&mut self, node: &str) -> bool {
    if !self.contains(node) {
        self.adjacency_table.entry(node.to_string()).or_insert_with(Vec::new);
        true
    } else {
        false
    }
}
```

#### 获取所有边
这个方法遍历邻接表，返回一个包含所有边的列表。

```rust
fn edges(&self) -> Vec<(&String, &String, i32)> {
    let mut edges = Vec::new();
    for (from_node, from_node_neighbours) in self.adjacency_table() {
        for (to_node, weight) in from_node_neighbours {
            edges.push((from_node, to_node, *weight));
        }
    }
    edges
}
```

### 单元测试
测试包括验证边的正确添加和查询功能。

```rust
#[test]
fn test_add_edge() {
    let mut graph = UndirectedGraph::new();
    graph.add_edge(("a", "b", 5));
    assert!(graph.edges().contains(&("a".to_string(), "b".to_string(), 5)));
    assert!(graph.edges().contains(&("b".to_string(), "a".to_string(), 5)));
}
```

通过这种方式，我们构建了一个基本的无向图结构，能够进行边和节点的添加，以及相关查询操作。

在解题中涉及到的图的概念和操作是算法和数据结构中的核心组成部分。这个问题的拓展讨论将围绕图的其他类型、算法以及应用场景进行。这些扩展内容可以帮助更深入地理解图结构的复杂性和灵活性。

### 扩展知识点：

1. **图的类型**：
   - **有向图**：与无向图相比，有向图的边具有方向性，这意味着每条边是从一个节点指向另一个节点。
   - **加权图**：图中的边赋予了权重，这在模拟和解决如最短路径等问题时非常有用。
   - **多重图**：允许两个节点之间有多条边和/或节点与自身连接的边。

2. **图的遍历算法**：
   - **深度优先搜索（DFS）**：一种用栈（通常是递归的函数调用栈）来探索尽可能深的分支的算法。
   - **广度优先搜索（BFS）**：一种用队列来按层次顺序访问节点的算法。

3. **图算法应用**：
   - **最短路径问题**：如 Dijkstra 算法、Bellman-Ford 算法。
   - **网络流问题**：如 Ford-Fulkerson 方法，用于计算网络最大流。
   - **连通性问题**：检查图中任意两个节点之间是否存在路径。

4. **图的存储**：
   - **邻接矩阵**：一个二维数组，其中的元素表示节点间是否存在边。
   - **邻接列表**：为每个节点维护一个列表，列表中包含其所有邻接的节点。
   - **边列表**：一个包含所有边的列表，每条边记录了两个节点和权重。

### 扩展解题方法：

对于本题中的图实现，可以采用以下几种方法来扩展和提高其功能：

1. **实现有向图**：
   修改 `add_edge` 方法，只在一个方向上添加边，而不是两个方向。

   ```rust
   fn add_edge_directed(&mut self, from: &str, to: &str, weight: i32) {
       self.adjacency_table.entry(from.to_string()).or_default().push((to.to_string(), weight));
   }
   ```

2. **实现权重的更新**：
   允许更新已存在边的权重或添加新边时检查是否已存在相同的边。

   ```rust
   fn update_or_add_edge(&mut self, from: &str, to: &str, new_weight: i32) {
       let neighbors = self.adjacency_table.entry(from.to_string()).or_default();
       if let Some((_, weight)) = neighbors.iter_mut().find(|(node, _)| node == to) {
           *weight = new_weight;
       } else {
           neighbors.push((to.to_string(), new_weight));
       }
   }
   ```

3. **增强查询功能**：
   提供更多的查询功能，如查找所有从给定节点出发可达的节点。

   ```rust
   fn find_all_reachable(&self, start: &str) -> HashSet<String> {
       let mut visited = HashSet::new();
       let mut stack = vec![start.to_string()];
       while let Some(node) = stack.pop() {
           if !visited.contains(&node) {
               visited.insert(node.clone());
               if let Some(neighbors) = self.adjacency_table.get(&node) {
                   for (neighbor, _) in neighbors {
                       stack.push(neighbor.clone());
                   }
               }
           }
       }
       visited
   }
   ```
