# Exercise 64

- Name: ```quiz3```
- Path: ```exercises/quiz3.rs```
#### Hint: 

To find the best solution to this challenge you're going to need to think back to your knowledge of traits, specifically Trait Bound Syntax -  you may also need this: `use std::fmt::Display;`.


---



```rust,editable
// quiz3.rs
//
// This quiz tests:
// - Generics
// - Traits
//
// An imaginary magical school has a new report card generation system written
// in Rust! Currently the system only supports creating report cards where the
// student's grade is represented numerically (e.g. 1.0 -> 5.5). However, the
// school also issues alphabetical grades (A+ -> F-) and needs to be able to
// print both types of report card!
//
// Make the necessary code changes in the struct ReportCard and the impl block
// to support alphabetical report cards. Change the Grade in the second test to
// "A+" to show that your changes allow alphabetical grades.
//
// Execute `rustlings hint quiz3` or use the `hint` watch subcommand for a hint.

// I AM NOT DONE

pub struct ReportCard {
    pub grade: f32,
    pub student_name: String,
    pub student_age: u8,
}

impl ReportCard {
    pub fn print(&self) -> String {
        format!("{} ({}) - achieved a grade of {}",
            &self.student_name, &self.student_age, &self.grade)
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn generate_numeric_report_card() {
        let report_card = ReportCard {
            grade: 2.1,
            student_name: "Tom Wriggle".to_string(),
            student_age: 12,
        };
        assert_eq!(
            report_card.print(),
            "Tom Wriggle (12) - achieved a grade of 2.1"
        );
    }

    #[test]
    fn generate_alphabetic_report_card() {
        // TODO: Make sure to change the grade here after you finish the exercise.
        let report_card = ReportCard {
            grade: 2.1,
            student_name: "Gary Plotter".to_string(),
            student_age: 11,
        };
        assert_eq!(
            report_card.print(),
            "Gary Plotter (11) - achieved a grade of A+"
        );
    }
}

```

---

本练习的目标是扩展一个报告卡生成系统，使其能够支持不同类型的成绩表示方式，包括数值型和字母型成绩。这要求我们理解并应用 Rust 的泛型和特质（Traits）。

### 相关知识点：

1. **泛型**：允许在定义函数、结构体、枚举或方法时使用泛型类型参数，从而提供代码的灵活性和重用性。
2. **特质**：特质定义了一组方法，可以为不同的数据类型实现。在 Rust 中，特质类似于其他语言的接口。
3. **特质约束**：在泛型中使用特质约束来指定泛型类型必须实现某些特质，这使得我们可以对这些类型调用特定的方法。
4. **Display 特质**：`std::fmt::Display` 是一个标准库特质，用于格式化输出。实现这个特质允许类型以用户友好的方式显示。

### 解题方法：

步骤 1: 修改 `ReportCard` 结构体，使其成绩 `grade` 字段使用泛型。
步骤 2: 为 `ReportCard` 结构体添加特质约束，确保 `grade` 字段的类型实现了 `Display` 特质，这样我们可以在 `print` 方法中打印它。
步骤 3: 调整 `print` 方法以适应泛型。
步骤 4: 修改测试用例以展示结构体能够处理不同类型的成绩。

### 代码示例：

```rust
use std::fmt::Display; // 引入 Display 特质

pub struct ReportCard<T: Display> {
    pub grade: T,
    pub student_name: String,
    pub student_age: u8,
}

impl<T: Display> ReportCard<T> {
    pub fn print(&self) -> String {
        format!("{} ({}) - achieved a grade of {}",
            &self.student_name, &self.student_age, &self.grade)
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn generate_numeric_report_card() {
        let report_card = ReportCard {
            grade: 2.1,
            student_name: "Tom Wriggle".to_string(),
            student_age: 12,
        };
        assert_eq!(
            report_card.print(),
            "Tom Wriggle (12) - achieved a grade of 2.1"
        );
    }

    #[test]
    fn generate_alphabetic_report_card() {
        let report_card = ReportCard {
            grade: "A+",
            student_name: "Gary Plotter".to_string(),
            student_age: 11,
        };
        assert_eq!(
            report_card.print(),
            "Gary Plotter (11) - achieved a grade of A+"
        );
    }
}
```

在这段代码中，`ReportCard` 现在是一个泛型结构体，可以接受任何实现了 `Display` 特质的类型作为成绩。我们为 `print` 方法实现了适当的格式化输出，确保无论成绩类型如何，都能正确显示。测试用例展示了该结构体如何适应不同的成绩表示方法，验证了代码的功能性。

这种方法不仅使代码更加通用，还保证了类型安全，确保只有那些能够被适当显示的类型才能被用作成绩，避免了运行时错误。
