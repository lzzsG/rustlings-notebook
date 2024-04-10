# Exercise 33

- Name: ```structs3```
- Path: ```exercises/structs/structs3.rs```
#### Hint: 

For is_international: What makes a package international? Seems related to the places it goes through right?

For get_fees: This method takes an additional argument, is there a field in the Package struct that this relates to?

Have a look in The Book, to find out more about method implementations: [https://doc.rust-lang.org/book/ch05-03-method-syntax.html](https://doc.rust-lang.org/book/ch05-03-method-syntax.html) ([Chinese version](https://rustwiki.org/zh-CN/book/ch05-03-method-syntax.html))


---



```rust,editable
// structs3.rs
//
// Structs contain data, but can also have logic. In this exercise we have
// defined the Package struct and we want to test some logic attached to it.
// Make the code compile and the tests pass!
//
// Execute `rustlings hint structs3` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

#[derive(Debug)]
struct Package {
    sender_country: String,
    recipient_country: String,
    weight_in_grams: i32,
}

impl Package {
    fn new(sender_country: String, recipient_country: String, weight_in_grams: i32) -> Package {
        if weight_in_grams <= 0 {
            panic!("Can not ship a weightless package.")
        } else {
            Package {
                sender_country,
                recipient_country,
                weight_in_grams,
            }
        }
    }

    fn is_international(&self) -> ??? {
        // Something goes here...
    }

    fn get_fees(&self, cents_per_gram: i32) -> ??? {
        // Something goes here...
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    #[should_panic]
    fn fail_creating_weightless_package() {
        let sender_country = String::from("Spain");
        let recipient_country = String::from("Austria");

        Package::new(sender_country, recipient_country, -2210);
    }

    #[test]
    fn create_international_package() {
        let sender_country = String::from("Spain");
        let recipient_country = String::from("Russia");

        let package = Package::new(sender_country, recipient_country, 1200);

        assert!(package.is_international());
    }

    #[test]
    fn create_local_package() {
        let sender_country = String::from("Canada");
        let recipient_country = sender_country.clone();

        let package = Package::new(sender_country, recipient_country, 1200);

        assert!(!package.is_international());
    }

    #[test]
    fn calculate_transport_fees() {
        let sender_country = String::from("Spain");
        let recipient_country = String::from("Spain");

        let cents_per_gram = 3;

        let package = Package::new(sender_country, recipient_country, 1500);

        assert_eq!(package.get_fees(cents_per_gram), 4500);
        assert_eq!(package.get_fees(cents_per_gram * 2), 9000);
    }
}

```

---

### 本题内容：

这个练习要求你在`Package`结构体上实现一些逻辑，具体包括：

- 判断一个包裹是否为国际邮寄（两个国家不同即为国际邮寄）。
- 根据包裹的重量和每克的费用计算邮寄费用。

### 相关知识点：

- **方法实现**：在Rust中，可以为结构体实现方法。方法是定义在`impl`块中的函数，它们可以访问结构体的字段。第一个参数总是`self`，它代表方法被调用的实例。

- **布尔逻辑**：在判断是否为国际邮寄时，需要使用到布尔逻辑。比较两个字符串是否相等，可以用`==`操作符。

- **计算与返回值**：计算邮寄费用涉及到基本的数学运算，并需要方法有一个返回值。

### 解题方法：

#### 实现`is_international`方法
- **步骤描述**：
  1. 方法的返回类型应该是`bool`。
  2. 比较`sender_country`和`recipient_country`字段，如果它们不相等，则包裹为国际邮寄。

#### 实现`get_fees`方法
- **步骤描述**：
  1. 方法的返回类型应该是`i32`。
  2. 使用包裹的重量（`weight_in_grams`）乘以每克的费用（`cents_per_gram`参数），计算并返回邮寄费用。

### 代码示例：

```rust
#[derive(Debug)]
struct Package {
    sender_country: String,
    recipient_country: String,
    weight_in_grams: i32,
}

impl Package {
    fn new(sender_country: String, recipient_country: String, weight_in_grams: i32) -> Package {
        if weight_in_grams <= 0 {
            panic!("Cannot ship a weightless package.")
        } else {
            Package {
                sender_country,
                recipient_country,
                weight_in_grams,
            }
        }
    }

    fn is_international(&self) -> bool {
        self.sender_country != self.recipient_country
    }

    fn get_fees(&self, cents_per_gram: i32) -> i32 {
        self.weight_in_grams * cents_per_gram
    }
}
```

通过完成这个练习，你将加深理解结构体如何封装数据和逻辑，并学习到如何为结构体实现方法，这是构建复杂Rust应用的基础。掌握这些概念对于高效地使用Rust来说非常重要。

### 测试相关内容：

测试是软件开发过程中一个关键的环节，用于确保代码的正确性和功能性满足预期。在Rust中，测试被用来验证函数或方法的行为，尤其是在边缘情况下的表现。`structs3`练习中的测试用例展示了如何使用Rust的测试框架来检验`Package`结构体及其方法的正确性。

#### 1. `#[cfg(test)]`注解：
这个属性指示编译器，随后的模块是一个测试模块，里面的内容只在执行`cargo test`命令时编译和运行。

#### 2. `#[test]`属性：
用于标记单元测试函数。每个带有`#[test]`属性的函数都会被作为单独的测试用例执行。

#### 3. `should_panic`属性：
某些情况下，我们期望函数在遇到错误条件时终止执行并抛出panic。`#[should_panic]`属性用于标记这些期望失败的测试，如果标记了该属性的测试函数panic了，那么这个测试就被视为通过。

#### 4. `assert!`宏：
用于断言测试中的条件是否为真。如果条件为假，则`assert!`宏会导致测试失败。

#### 5. `assert_eq!`和`assert_ne!`宏：
这两个宏用于检查两个值是否相等或不相等。如果检查失败（即值不符合预期），测试也会失败。

#### 6. 在`structs3`练习的测试中：

- **测试包裹创建**：`create_international_package`和`create_local_package`两个测试用例通过检查包裹的`is_international`方法的返回值，来验证是否正确地判断了包裹是国际邮寄还是本地邮寄。

- **测试邮费计算**：`calculate_transport_fees`测试用例通过调用`get_fees`方法并传入不同的每克费用，来验证邮费计算是否正确。

- **测试无效数据**：`fail_creating_weightless_package`测试用例期望在试图创建一个重量为负数的包裹时抛出panic，体现了通过测试确保函数对无效输入的处理方式符合预期。

通过这些测试，我们不仅能验证`Package`结构体的逻辑正确性，还能确保其在各种条件下的稳定性和可靠性。编写全面的测试是提高代码质量的重要手段，Rust的测试工具提供了强大的支持来帮助我们实现这一点。

## 扩展知识点与解答：

在完成`structs3`练习后，你已经了解到结构体不仅可以存储数据，还可以包含逻辑。这个练习是一个很好的起点，用于探索Rust中结构体的更多高级特性和使用场景。以下是一些相关的扩展知识点和解题方法，它们将帮助你更深入地理解和运用Rust的结构体。

### 扩展知识点：

1. **生命周期注解**：
   - 当结构体中包含引用时，你需要使用生命周期注解来确保结构体实例不会比它所包含的数据活得更久。生命周期注解帮助Rust的借用检查器理解引用之间的关系，确保数据安全。

2. **特质实现（Trait Implementation）**：
   - 除了定义方法，你还可以为结构体实现特质（Traits）。这允许你为结构体提供标准的行为，例如允许结构体实例之间的比较、复制或输出格式化。

3. **模式匹配和结构体**：
   - 结构体可以与Rust的模式匹配特性结合使用，这在你需要根据结构体字段的不同值执行不同操作时特别有用。

### 扩展解题方法：

1. **引入生命周期注解**：
   - 如果你的结构体需要存储引用，考虑引入生命周期参数。这需要你在结构体定义以及相关方法实现中标注生命周期，以确保数据的有效性。

2. **为结构体实现特质**：
   - 例如，通过实现`Display`特质，可以自定义结构体实例的打印输出。为`Package`结构体实现这个特质，可以让你直接通过`println!`宏以友好的方式输出包裹信息。

3. **利用模式匹配增强逻辑**：
   - 在结构体的方法中使用模式匹配来根据字段值的不同选择不同的执行路径。这可以使代码更加灵活和强大。

#### 示例代码：

假设你要为`Package`结构体实现`Display`特质以自定义其打印输出：

```rust
use std::fmt;

impl fmt::Display for Package {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "Package from {} to {}, Weight: {}g", 
        self.sender_country, self.recipient_country, self.weight_in_grams)
    }
}
```

在实现特质时，你可以访问结构体的字段来构建输出字符串。通过这样的方式，你可以为结构体提供更丰富的行为和接口，使其更加强大和灵活。
