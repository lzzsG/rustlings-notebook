# Exercise 61

- Name: ```traits3```
- Path: ```exercises/traits/traits3.rs```
#### Hint: 

Traits can have a default implementation for functions. Structs that implemen the trait can then use the default version of these functions if they choose not implement the function themselves.

See the documentation at: [https://doc.rust-lang.org/book/ch10-02-traits.html#default-implementations](https://doc.rust-lang.org/book/ch10-02-traits.html#default-implementations) ([Chinese version](https://rustwiki.org/zh-CN/book/ch10-02-traits.html#%E9%BB%98%E8%AE%A4%E5%AE%9E%E7%8E%B0))



---



```rust,editable
// traits3.rs
//
// Your task is to implement the Licensed trait for both structures and have
// them return the same information without writing the same function twice.
//
// Consider what you can add to the Licensed trait.
//
// Execute `rustlings hint traits3` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

pub trait Licensed {
    fn licensing_info(&self) -> String;
}

struct SomeSoftware {
    version_number: i32,
}

struct OtherSoftware {
    version_number: String,
}

impl Licensed for SomeSoftware {} // Don't edit this line
impl Licensed for OtherSoftware {} // Don't edit this line

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn is_licensing_info_the_same() {
        let licensing_info = String::from("Some information");
        let some_software = SomeSoftware { version_number: 1 };
        let other_software = OtherSoftware {
            version_number: "v2.0.0".to_string(),
        };
        assert_eq!(some_software.licensing_info(), licensing_info);
        assert_eq!(other_software.licensing_info(), licensing_info);
    }
}

```

---

### 本题内容

本练习要求实现一个名为 `Licensed` 的特质，并为两个结构体 `SomeSoftware` 和 `OtherSoftware` 提供实现。关键在于使用特质的默认实现，避免为每个结构体重复编写相同的功能代码。

### 相关知识点

1. **特质（Traits）**: 特质是 Rust 中定义共享行为的一种方式，类似于其他语言中的接口。
2. **默认实现**: Rust 允许在特质中为某些或全部方法提供默认实现。实现该特质的结构体可以选择使用这些默认实现，也可以提供自己的实现来覆盖它。
3. **结构体（Structures）**: Rust 中的数据结构，用于封装相关联的数据。

### 解题方法

1. **添加默认实现**：在 `Licensed` 特质中添加一个默认实现的方法 `licensing_info`，返回统一的授权信息。
2. **特质实现**：为 `SomeSoftware` 和 `OtherSoftware` 结构体实现 `Licensed` 特质，这些实现将自动使用默认方法，除非明确提供其他实现。

### 代码示例

以下是完成任务的示例代码：

```rust
pub trait Licensed {
    // 提供默认实现
    fn licensing_info(&self) -> String {
        String::from("Some information")
    }
}

struct SomeSoftware {
    version_number: i32,
}

struct OtherSoftware {
    version_number: String,
}

// 为SomeSoftware和OtherSoftware实现Licensed特质
impl Licensed for SomeSoftware {}
impl Licensed for OtherSoftware {}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn is_licensing_info_the_same() {
        let licensing_info = String::from("Some information");
        let some_software = SomeSoftware { version_number: 1 };
        let other_software = OtherSoftware {
            version_number: "v2.0.0".to_string(),
        };
        assert_eq!(some_software.licensing_info(), licensing_info);
        assert_eq!(other_software.licensing_info(), licensing_info);
    }
}
```

### 分析

- **特质的默认实现** 使得每个实现 `Licensed` 的结构体都可以不用编写重复的 `licensing_info` 方法。
- **简洁性与复用**：通过特质的默认实现，我们避免了在每个结构体中重复相同代码的需求，减少了冗余并提高了代码的可维护性。

通过这种方法，我们可以看到 Rust 中特质默认实现的强大功能，特别是在处理多个类型需要共享相同行为时，使用默认实现可以极大地简化代码和提高代码复用。

## 扩展知识点与解答：

### 扩展知识点

#### 1. 默认实现与行为重载
默认实现允许在特质中为某些或所有方法提供基本的实现。结构体或枚举类型实现这个特质时，可以选择使用这些默认实现，或者提供一个自定义实现来覆盖它。

- **灵活性**：默认实现提供了极大的灵活性，使库的设计者可以为使用者提供一个稳固且易于扩展的基础。
- **易用性**：为那些可能不需要自定义每一个方法的用户提供了便利，用户只需关心他们特别感兴趣的方法。

#### 2. 特质继承
在 Rust 中，特质还可以继承其他特质。这意味着你可以创建一个基础特质，提供一些通用的方法，然后通过其他特质扩展这些功能。

- **模块化设计**：通过特质继承，可以构建出层次化和模块化的接口，从而增强代码的组织性和可读性。

#### 3. 关联类型
特质可以包含关联类型，这是在特质内部定义的类型占位符，允许在实现特质时指定这些类型。

- **类型约束**：关联类型提供了一种将特定类型与特质实现相关联的方式，这对于创建复杂的泛型接口非常有用。

### 扩展解题方法

#### 1. 利用默认实现简化代码
在实现多个具有共通行为的类型时，考虑在特质中提供默认方法实现，这样可以减少代码重复，并简化各类型的实现。例如，可以为日志记录、错误处理或简单的数据检索提供默认实现。

```rust
trait Logger {
    fn log(&self, message: &str) {
        println!("Default log: {}", message);
    }
}

struct MyApp;
impl Logger for MyApp {}

struct MyService;
impl Logger for MyService {}
```

#### 2. 特质继承实现层次化接口
设计接口时，通过特质继承创建基本特质，然后扩展这些特质以添加更具体的功能。

```rust
trait Animal {
    fn eat(&self);
}

trait Mammal: Animal {
    fn speak(&self);
}

struct Human;
impl Animal for Human {
    fn eat(&self) {
        println!("Eating food.");
    }
}
impl Mammal for Human {
    fn speak(&self) {
        println!("Hello!");
    }
}
```

#### 3. 利用关联类型提高接口的表达力
在特质中使用关联类型来定义更复杂的行为模式，如数据转换或迭代器。

```rust
trait Transformer {
    type Input;
    type Output;
    fn transform(&self, input: Self::Input) -> Self::Output;
}

struct StringToInt;
impl Transformer for StringToInt {
    type Input = String;
    type Output = i32;
    
    fn transform(&self, input: Self::Input) -> Self::Output {
        input.parse().unwrap_or(0)
    }
}
```

通过这些方法，可以创建出功能丰富、灵活性高、易于维护的 Rust 应用和库。使用默认实现和特质继承可以大大减少重复代码并提高项目的可扩展性。同时，关联类型则为特质提供了强大的类型安全性和表达力。

---

## 插入：关于trait相关的引用、指针、所有权以及借用

在 Rust 中使用特质（traits）涉及到对引用、指针、所有权以及借用等核心概念的理解。这些概念对于确保代码的安全性和效率至关重要。当涉及到特质方法的签名时，正确地使用 `self`、`&self` 和 `&mut self` 是非常重要的。

### 1. `Self` vs. `&self` vs. `&mut self`

- **`Self`** 是一个类型占位符，代表实现该特质的类型本身。如果一个方法接受 `self`，它通常意味着这个方法在调用后会消耗（take ownership of）该类型的实例。

- **`&self`** 表示方法通过不可变引用借用实例，这意味着方法内部不能修改实例的状态，但可以读取实例的数据。这是最常见的用法，因为它允许同时有多个借用，提高了灵活性而不牺牲安全。

- **`&mut self`** 表示方法需要一个可变引用到实例。这允许方法修改实例的状态。可变引用的规则更严格，同一时间只能有一个可变引用存在，或者多个不可变引用，两者不能同时存在。

### 2. 特质对象与动态分派

特质对象如 `&dyn Trait` 或 `Box<dyn Trait>` 是使用动态分派的特质引用。这些对象背后使用了指向数据和虚表（vtable）的指针，允许在运行时解析方法调用。

- **`items: &[&dyn Drawable]`** 示例中，`items` 是一个切片，其中的元素是指向实现了 `Drawable` 特质的类型的动态引用。这种结构允许在同一个集合中存储不同类型的对象，只要它们都实现了 `Drawable` 特质。

### 3. 引用和借用的重要性

在 Rust 中，正确管理引用和借用对于保证内存安全和防止数据竞争至关重要。通过特质方法的签名，我们可以控制函数如何访问和修改数据：

```rust
trait Logger {
    fn log(&self, message: &str) {
        println!("Default log: {}", message);
    }
}
```

在这个例子中：
- `fn log(&self, message: &str)` 表示 `log` 方法通过不可变引用借用实现了 `Logger` 特质的类型的实例。这意味着 `log` 方法可以在不获取所有权的情况下读取实例的状态，并且同一时间可以有多个这样的引用存在，促进了数据的共享而不牺牲安全。

## 对比 `Self`, `&self`, 和 `&mut self`

我们将使用一个简单的 `Rectangle` 结构体和一个 `Shape` 特质来展示这三种不同的方法签名的使用：

```rust
trait Shape {
    fn area(self) -> f64; //面积
    fn scale(&mut self, factor: f64); //缩放
    fn describe(&self) -> String; //描述
}

struct Rectangle {
    width: f64,
    height: f64,
}

impl Shape for Rectangle {
    // 使用 Self，这个方法需要获取所有权
    fn area(self) -> f64 {
        self.width * self.height
    }

    // 使用 &mut self，这个方法需要可变引用，以修改实例
    fn scale(&mut self, factor: f64) {
        self.width *= factor;
        self.height *= factor;
    }

    // 使用 &self，这个方法不会修改实例，只需读取
    fn describe(&self) -> String {
        format!("Rectangle with width {} and height {}", self.width, self.height)
    }
}

fn main() {
    let mut rect = Rectangle { width: 3.0, height: 4.0 };

    // 调用 describe，只需要不可变引用
    println!("Before scaling: {}", rect.describe());

    // 调用 scale，需要可变引用
    rect.scale(2.0);
    println!("After scaling: {}", rect.describe());

    // 调用 area，需要所有权，所以我们传递 rect 并移动所有权
    let area = rect.area();
    println!("Area: {}", area);

    // 此时 rect 不再可用，因为它的所有权已经被 area 方法获取
}
```

### 解释：

1. **`Self`**:
   - 在 `area` 方法中使用 `Self` 类型，这意味着调用该方法需要拥有 `Rectangle` 的所有权。这样做的方法在调用后将消耗其输入对象，使其不能再次使用，除非方法返回该类型的实例。

2. **`&self`**:
   - 在 `describe` 方法中使用 `&self` 表示该方法只需通过不可变引用访问调用者。这是最常用的方法签名，因为它允许多个地方同时借用对象，而不会引起状态的改变。

3. **`&mut self`**:
   - `scale` 方法使用 `&mut self`，表示它需要一个可变引用来修改对象。这表示调用 `scale` 方法时，没有其他活跃的不可变或可变借用，这样能确保在修改数据时不会发生数据竞争。

这个示例展示了如何根据方法的实际需求选择合适的自引用类型。使用 `Self`、`&self` 或 `&mut self` 取决于你是否需要所有权，是否需要修改对象，或者只是需要读取对象的数据。理解这些区别对于写出正确和高效的 Rust 代码非常重要。

---

## 关于mut self

在 Rust 中，`mut self` 在结构体的具体实现中的使用允许方法消耗并修改它的调用者，然后返回这个被修改的调用者。这种用法非常适合那些需要修改自身状态并且想要支持方法链式调用的场景，如构建器模式（Builder pattern）或者配置对象。

然而，重要的是要注意，在特质（trait）的方法声明中，我们不能使用 `mut self`。

**尝试运行下面的<span style="color: red;">错误</span>代码**

```rust
trait Use {
    fn use_item(mut self) -> String;
}

struct Item {
    uses: u32,
}

impl Item {
    fn new(uses: u32) -> Self {
        Item { uses }
    }
}

impl Use for Item {
    fn use_item(mut self) -> String {
        if self.uses > 0 {
            self.uses -= 1;
            format!("Item used, {} uses left", self.uses)
        } else {
            format!("No uses left")
        }
    }
}

fn main() {
    let item = Item::new(3);
    let message = item.use_item();
    println!("{}", message);
}

```



### 示例：使用 `mut self`

下面是一个正确的使用 `mut self` 的示例，展示了一个构建者模式的实现，这种模式允许我们连续设置属性然后构建一个对象。

#### 定义结构体和实现

```rust
struct CarBuilder {
    color: String,
    horsepower: u32,
    navigation: bool,
}

impl CarBuilder {
    // 构造函数，初始化一个新的 CarBuilder
    fn new() -> Self {
        CarBuilder {
            color: "Black".to_string(),
            horsepower: 120,
            navigation: false,
        }
    }

    // 设置颜色
    fn color(mut self, color: &str) -> Self {
        self.color = color.to_string();
        self
    }

    // 设置马力
    fn horsepower(mut self, horsepower: u32) -> Self {
        self.horsepower = horsepower;
        self
    }

    // 启用或禁用导航系统
    fn navigation(mut self, navigation: bool) -> Self {
        self.navigation = navigation;
        self
    }

    // 构建最终的 Car 对象
    fn build(self) -> Car {
        Car {
            color: self.color,
            horsepower: self.horsepower,
            navigation: self.navigation,
        }
    }
}

// 一个简单的 Car 结构体来接收构建结果
struct Car {
    color: String,
    horsepower: u32,
    navigation: bool,
}

fn main() {
    let car = CarBuilder::new()
        .color("Red")
        .horsepower(250)
        .navigation(true)
        .build();

    println!("Car details: {} HP, {} color, Navigation: {}", car.horsepower, car.color, car.navigation);
}
```

### 总结

- `mut self` 在具体类型的方法实现中是有效的，适用于需要修改并返回实例的场景，支持如构建者模式中的链式方法调用。
- 在特质方法声明中，不能使用 `mut self`，因为这会导致编译错误。在特质中，你应该使用 `&mut self` 来修改状态，或 `&self` 进行读取操作。
- 上面的 `CarBuilder` 示例清晰地展示了如何在实践中使用 `mut self` 来构建可链式调用的方法，这在很多现代 Rust 应用程序中都是一种非常实用的模式。
