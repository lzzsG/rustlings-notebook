# Exercise 46

- Name: ```hashmaps3```
- Path: ```exercises/hashmaps/hashmaps3.rs```
#### Hint: 

Hint 1: Use the `entry()` and `or_insert()` methods of `HashMap` to insert entries corresponding to each team in the scores table.

Learn more at [https://doc.rust-lang.org/stable/book/ch08-03-hash-maps.html#only-inserting-a-value-if-the-key-has-no-value](https://doc.rust-lang.org/stable/book/ch08-03-hash-maps.html#only-inserting-a-value-if-the-key-has-no-value) ([Chinese version](https://rustwiki.org/zh-CN/book/ch08-03-hash-maps.html#%E5%8F%AA%E5%9C%A8%E9%94%AE%E6%B2%A1%E6%9C%89%E5%AF%B9%E5%BA%94%E5%80%BC%E6%97%B6%E6%8F%92%E5%85%A5))

Hint 2: If there is already an entry for a given key, the value returned by `entry()` can be updated based on the existing value.

Learn more at [https://doc.rust-lang.org/book/ch08-03-hash-maps.html#updating-a-value-based-on-the-old-value](https://doc.rust-lang.org/book/ch08-03-hash-maps.html#updating-a-value-based-on-the-old-value) ([Chinese version](https://rustwiki.org/zh-CN/book/ch08-03-hash-maps.html#%E6%A0%B9%E6%8D%AE%E6%97%A7%E5%80%BC%E6%9B%B4%E6%96%B0%E4%B8%80%E4%B8%AA%E5%80%BC))



---



```rust,editable
// hashmaps3.rs
//
// A list of scores (one per line) of a soccer match is given. Each line is of
// the form : "<team_1_name>,<team_2_name>,<team_1_goals>,<team_2_goals>"
// Example: England,France,4,2 (England scored 4 goals, France 2).
//
// You have to build a scores table containing the name of the team, goals the
// team scored, and goals the team conceded. One approach to build the scores
// table is to use a Hashmap. The solution is partially written to use a
// Hashmap, complete it to pass the test.
//
// Make me pass the tests!
//
// Execute `rustlings hint hashmaps3` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

use std::collections::HashMap;

// A structure to store the goal details of a team.
struct Team {
    goals_scored: u8,
    goals_conceded: u8,
}

fn build_scores_table(results: String) -> HashMap<String, Team> {
    // The name of the team is the key and its associated struct is the value.
    let mut scores: HashMap<String, Team> = HashMap::new();

    for r in results.lines() {
        let v: Vec<&str> = r.split(',').collect();
        let team_1_name = v[0].to_string();
        let team_1_score: u8 = v[2].parse().unwrap();
        let team_2_name = v[1].to_string();
        let team_2_score: u8 = v[3].parse().unwrap();
        // TODO: Populate the scores table with details extracted from the
        // current line. Keep in mind that goals scored by team_1
        // will be the number of goals conceded from team_2, and similarly
        // goals scored by team_2 will be the number of goals conceded by
        // team_1.
    }
    scores
}

#[cfg(test)]
mod tests {
    use super::*;

    fn get_results() -> String {
        let results = "".to_string()
            + "England,France,4,2\n"
            + "France,Italy,3,1\n"
            + "Poland,Spain,2,0\n"
            + "Germany,England,2,1\n";
        results
    }

    #[test]
    fn build_scores() {
        let scores = build_scores_table(get_results());

        let mut keys: Vec<&String> = scores.keys().collect();
        keys.sort();
        assert_eq!(
            keys,
            vec!["England", "France", "Germany", "Italy", "Poland", "Spain"]
        );
    }

    #[test]
    fn validate_team_score_1() {
        let scores = build_scores_table(get_results());
        let team = scores.get("England").unwrap();
        assert_eq!(team.goals_scored, 5);
        assert_eq!(team.goals_conceded, 4);
    }

    #[test]
    fn validate_team_score_2() {
        let scores = build_scores_table(get_results());
        let team = scores.get("Spain").unwrap();
        assert_eq!(team.goals_scored, 0);
        assert_eq!(team.goals_conceded, 2);
    }
}

```

---

### 本题内容

本练习的目标是使用 `HashMap` 来构建一个足球比赛的得分表。你需要处理一系列的比赛结果，每个结果包括两个队伍的名字和各自的进球数。这个练习将帮助学生了解如何在 Rust 中使用 `HashMap` 来存储和更新键值对数据，同时也涉及对数据结构的查询和更新。

### 相关知识点

1. **`HashMap` 使用**：了解如何创建和使用 `HashMap`，包括插入、更新和查询操作。
2. **结构体**：通过定义 `Team` 结构体来组织数据，每个队伍的进球数和失球数作为结构体的字段。
3. **字符串处理**：从字符串中解析数据，涉及字符串分割和类型转换。

### 解题方法

#### 步骤描述：

1. **解析输入数据**：使用 `split` 方法分割每一行的数据以获取队伍名称和得分。
2. **更新 `HashMap`**：对每一场比赛的两个队伍，检查 `HashMap` 中是否已存在相关记录，若不存在则插入，若存在则更新。
3. **记录进球和失球**：对于每个队伍，更新它的进球数和失球数。进球数直接从分割得到的数据中读取，失球数则是对方的进球数。

#### 代码示例：

这里是完善的 `build_scores_table` 函数的代码示例：

```rust
use std::collections::HashMap;

struct Team {
    goals_scored: u8,
    goals_conceded: u8,
}

fn build_scores_table(results: String) -> HashMap<String, Team> {
    let mut scores: HashMap<String, Team> = HashMap::new();

    for r in results.lines() {
        let v: Vec<&str> = r.split(',').collect();
        let team_1_name = v[0].to_string();
        let team_1_score: u8 = v[2].parse().unwrap();
        let team_2_name = v[1].to_string();
        let team_2_score: u8 = v[3].parse().unwrap();

        // Update team 1
        let team_1 = scores.entry(team_1_name).or_insert(Team {
            goals_scored: 0,
            goals_conceded: 0,
        });
        team_1.goals_scored += team_1_score;
        team_1.goals_conceded += team_2_score;

        // Update team 2
        let team_2 = scores.entry(team_2_name).or_insert(Team {
            goals_scored: 0,
            goals_conceded: 0,
        });
        team_2.goals_scored += team_2_score;
        team_2.goals_conceded += team_1_score;
    }
    scores
}
```

### 说明：

- **`entry().or_insert()`**：这个方法是用来处理 `HashMap` 中键值对的一种高效方式。如果键不存在，它会插入默认值；如果存在，则返回现有值的可变引用。
- **累加数据**：对于每个队伍，我们通过累加其进球数和失球数来更新其统计数据。

通过这种方式，学生不仅能学习如何使用 `HashMap`，还能理解如何通过结构体来管理和更新复杂的数据结构。这对于处理实际应用中的各种数据统计和管理任务非常有帮助。

## 扩展知识点与解答：

这个`hashmaps3`练习展示了如何使用`HashMap`来处理和记录复杂数据集合的一种实际应用。为了完成这个练习，我们深入了解了几个关键的`HashMap`方法，以及如何高效地使用它们来更新和管理数据。以下是扩展解答和知识点的总结：

### 扩展知识点：

1. **`entry()` 方法**：`entry()` 方法用于获取集合中键的入口（`Entry`）。它返回一个`Entry`枚举，该枚举可能是`Occupied`（已占用，键存在）或`Vacant`（空，键不存在）。

2. **`or_insert()` 方法**：与`entry()`方法一起使用，`or_insert()`可以在键不存在时插入一个默认值。如果键已存在，它则返回对现有值的可变引用。

3. **`and_modify()` 方法**：这个方法允许我们在键已存在时对其值进行修改。它通常与`or_insert()`结合使用，以实现“插入或更新”模式。

### 扩展解题方法：

在这个练习中，我们需要根据比赛结果更新每个球队的进球数和失球数。为了高效地实现这一点，我们可以利用`entry()`和`or_insert()`方法来确保每个球队在`HashMap`中有一个对应的记录。如果某个球队的记录还不存在，我们就插入一个新记录。然后，使用`and_modify()`方法更新已存在记录的进球数和失球数。

### 示例代码：

```rust
use std::collections::HashMap;

struct Team {
    goals_scored: u8,
    goals_conceded: u8,
}

fn build_scores_table(results: String) -> HashMap<String, Team> {
    let mut scores: HashMap<String, Team> = HashMap::new();

    for r in results.lines() {
        let v: Vec<&str> = r.split(',').collect();
        let (team_1_name, team_2_name, team_1_score, team_2_score) = 
            (v[0].to_string(), v[1].to_string(), v[2].parse().unwrap(), v[3].parse().unwrap());

        scores.entry(team_1_name)
            .and_modify(|e| { 
                e.goals_scored += team_1_score;
                e.goals_conceded += team_2_score;
            })
            .or_insert(Team { goals_scored: team_1_score, goals_conceded: team_2_score });

        scores.entry(team_2_name)
            .and_modify(|e| {
                e.goals_scored += team_2_score;
                e.goals_conceded += team_1_score;
            })
            .or_insert(Team { goals_scored: team_2_score, goals_conceded: team_1_score });
    }

    scores
}
```

### 重要概念：

- **高效数据管理**：这个练习展示了`HashMap`在管理和更新数据集时的高效性。通过利用`entry()`、`or_insert()`和`and_modify()`方法，我们可以确保数据的一致性和准确性，同时避免不必要的重复检查。
  
- **灵活的数据更新模式**：`entry()`方法提供了一种灵活的方式来处理可能不存在的键。这使我们能够在单个表达式中完成插入和更新操作，提高了代码的可读性和维护性。
