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

**图的基础结构**

我们使用 `HashMap` 来存储图的邻接表，每个键值对应一个节点和一个向量，该向量包含与该节点相连的其他节点及其边的权重。

```rust
pub struct UndirectedGraph {
    adjacency_table: HashMap<String, Vec<(String, i32)>>,
}
```

### 1. 在 `Graph` 特质中定义通用接口

- `Graph` 特质应该定义所有图结构共有的行为，例如添加节点和边。这里可以提供接口和部分默认实现：

```rust
pub trait Graph {
    fn new() -> Self;
    fn adjacency_table_mutable(&mut self) -> &mut HashMap<String, Vec<(String, i32)>>;
    fn adjacency_table(&self) -> &HashMap<String, Vec<(String, i32)>>;
    
    fn add_node(&mut self, node: &str) -> bool {
        let table = self.adjacency_table_mutable();
        if !table.contains_key(node) {
            table.insert(node.to_string(), Vec::new());
            true
        } else {
            false
        }
    }

    fn add_edge(&mut self, edge: (&str, &str, i32));

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

```

在这个实现中，`add_node` 方法检查是否已经存在节点，如果不存在则添加到邻接表中，并返回 `true` 表示成功添加；如果节点已存在，返回 `false`。

### 2. 在 `UndirectedGraph` 结构体中具体实现 `add_edge`
由于不同类型的图（无向图、有向图）添加边的方式可能不同（例如无向图需要添加两条边，有向图只需一条），因此在具体的图实现中定义 `add_edge` 方法是合适的：

```rust
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
        let (from, to, weight) = edge;
        self.adjacency_table_mutable().entry(from.to_string()).or_insert_with(Vec::new).push((to.to_string(), weight));
        self.adjacency_table_mutable().entry(to.to_string()).or_insert_with(Vec::new).push((from.to_string(), weight));
    }
}
```
这里，`add_edge` 方法为无向图添加边，因此在邻接表中为两个方向都添加了边。使用 `entry` API 来检查和插入节点，确保即使节点不存在也可以添加边。

通过这种方式，您可以在 `Graph` 特质中定义通用行为和接口，同时在具体的图结构实现（如 `UndirectedGraph`）中提供特定的实现细节。这种设计增加了代码的灵活性和可扩展性，使得添加更多类型的图（如有向图等）时更为简单和直接。

---

## 代码讲解

这段代码展示了如何在 Rust 中定义和实现一个无向图数据结构，包括对节点和边的管理。下面逐部分详细解释代码：

### 导入和错误类型定义

```rust
use std::collections::{HashMap, HashSet};
use std::fmt;
```
- **HashMap** 和 **HashSet** 用于存储图的邻接表和节点集合。
- **fmt** 用于实现错误信息的格式化。

```rust
#[derive(Debug, Clone)]
pub struct NodeNotInGraph;
impl fmt::Display for NodeNotInGraph {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "accessing a node that is not in the graph")
    }
}
```
- **NodeNotInGraph** 是一个自定义错误类型，用于表示尝试访问图中不存在的节点。
- **fmt::Display** 为 **NodeNotInGraph** 提供了自定义的显示格式化，使得错误信息更加友好。

### 无向图的实现

```rust
pub struct UndirectedGraph {
    adjacency_table: HashMap<String, Vec<(String, i32)>>,
}
```
- **UndirectedGraph** 结构体定义了无向图的主体，其中 **adjacency_table** 用来存储图的邻接表，键为节点名，值为与该节点相连的其他节点列表及边的权重。

```rust
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
        let (from, to, weight) = edge;
        self.adjacency_table_mutable()
            .entry(from.to_string())
            .or_insert_with(Vec::new)
            .push((to.to_string(), weight));
        self.adjacency_table_mutable()
            .entry(to.to_string())
            .or_insert_with(Vec::new)
            .push((from.to_string(), weight));
    }
}
```
- **Graph** 特质为 **UndirectedGraph** 提供了实现方法。
- **new** 方法创建一个新的图实例。
- **adjacency_table_mutable** 和 **adjacency_table** 方法分别提供对邻接表的可变和不可变引用。
- **add_edge** 方法添加一条边，同时更新两个节点的邻接信息，确保无向图的特性（即无向边在两个方向上都要表示）。

### 图特质

```rust
pub trait Graph {
    fn new() -> Self;
    fn adjacency_table_mutable(&mut self) -> &mut HashMap<String, Vec<(String, i32)>>;
    fn adjacency_table(&self) -> &HashMap<String, Vec<(String, i32)>>;
    fn add_node(&mut self, node: &str) -> bool {
        let table = self.adjacency_table_mutable();
        if !table.contains_key(node) {
            table.insert(node.to_string(), Vec::new());
            true
        } else {
            false
        }
    }
    fn add_edge(&mut self, edge: (&str, &str, i32));
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
```
- **Graph** 特质定义了图的基本功能，包括创建图、添加节点、添加边、检查节点存在性、获取所有节点和边。
- **add_node** 方法检查节点是否已在图中，如果不在则添加，并返回一个布尔值表示操作成功与否。
- **contains** 方法检查一个节点是否在图中。

- **nodes** 方法返回图中所有节点的集合。

- **edges** 方法收集并返回图中所有的边信息。

  

**`adjacency_table_mutable` 和 `adjacency_table`**，定义在 `Graph` 特质中并在 `UndirectedGraph` 结构体实现中具体化，提供对无向图的邻接表的可变和不可变访问。这是常见的设计模式，用于控制数据的访问权限，并保持数据的封装性和安全性。

### `adjacency_table_mutable`

```rust
fn adjacency_table_mutable(&mut self) -> &mut HashMap<String, Vec<(String, i32)>> {
    &mut self.adjacency_table
}
```

- **目的**：这个方法提供了一个对图的邻接表的**可变引用**。可变引用允许调用者修改邻接表，例如添加或移除边、修改边的权重等。
- **用法**：这个方法在需要修改图结构时被调用，如在 `add_edge` 方法中使用。因为修改操作需要改变内存中的数据，所以需要可变访问权限。

### `adjacency_table`

```rust
fn adjacency_table(&self) -> &HashMap<String, Vec<(String, i32)>> {
    &self.adjacency_table
}
```

- **目的**：提供对图的邻接表的**不可变引用**。不可变引用禁止对邻接表的任何修改，只允许进行读取操作。
- **用法**：这个方法适用于那些只需读取图数据而不需要修改它的情况，例如检查图中是否包含某个节点、获取图的所有节点或边等。

通过这两个方法，`UndirectedGraph` 保持了对其数据的严格控制，使得数据操作既安全又有效。这种设计模式体现了 Rust 语言的一大优势，即通过编译时检查保证内存安全和数据一致性。

---

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

