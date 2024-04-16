# smart_pointers

---

在 Rust 的智能指针练习系列中，我们将探讨如何有效利用 Rust 中的智能指针来管理内存、实现数据共享和优化数据结构的设计。以下是对几个重要练习的简要介绍，帮助你在开始具体练习前有所准备。

### 练习 77：box1

在这个练习中，你将学习如何使用 `Box<T>` 处理递归数据结构。`Box` 是一个将数据存储在堆上的智能指针，允许 Rust 在编译时处理那些大小未知的类型，特别是对于递归类型非常有用。本练习将引导你实现一个基本的链表结构（cons list），这是函数式编程语言中的常见数据结构。

**关键技能**：理解如何通过 `Box` 实现递归类型，以及为什么在递归数据结构中需要使用堆内存。

### 练习 78：rc1

这个练习将引入 `Rc<T>`，即引用计数智能指针，它使得程序中多个部分可以同时拥有某个数据的所有权，而且当最后一个所有者离开作用域时，数据会被自动清理。通过模拟太阳系中行星围绕太阳公转的模型，你将学习如何使用 `Rc<T>` 来共享数据。

**关键技能**：掌握 `Rc<T>` 在多所有者场景中的使用，了解如何通过引用计数来管理内存。

### 练习 79：arc1

这个练习转向并发编程，探讨 `Arc<T>`，即原子引用计数智能指针。`Arc<T>` 类似于 `Rc<T>`，但它是为了满足线程安全的需求而设计的。在这个任务中，你将使用 `Arc<T>` 来在多个线程之间安全地共享一个数字数组，每个线程将计算数组中一部分数据的和。

**关键技能**：学习 `Arc<T>` 的使用，特别是在多线程环境中共享数据时如何保持线程安全。

### 练习 80：cow1

`Cow<T>`，或称写时复制（Copy-On-Write），是一个非常有用的智能指针，允许你以借用的方式使用数据，直到你真正需要修改它为止。这个练习将通过多个单元测试来展示 `Cow<T>` 在何时复制数据上的智能行为。

**关键技能**：了解 `Cow<T>` 的概念，包括其如何延迟数据复制以及在何种场景下使用它最为有效。

通过上述练习，你不仅能够加深对 Rust 智能指针的理解，还能学习到如何在具体的编程任务中应用这些知识，以优化代码的内存管理和性能。

---

```rust
// 你不必现在理解以下代码，不过你可以尝试运行它。

use std::rc::Rc;
use std::sync::Arc;
use std::thread;
use std::borrow::Cow;

enum SpellComponent {
    Element(String),
    Energy(Box<SpellComponent>),
}

use SpellComponent::{Element, Energy};

struct SpellBook {
    components: Rc<Vec<String>>,
}

struct MagicCircle {
    energies: Arc<Vec<String>>,
}

fn main() {
    let _spell_component = Energy(Box::new(Element("Fire".to_string())));

    let spell_book = SpellBook {
        components: Rc::new(vec!["Fire".to_string(), "Water".to_string(), "Earth".to_string()]),
    };

    let shared_components = Rc::clone(&spell_book.components);
    println!("Ferris uses his spell components: {:?}", shared_components);

    let spell_components_for_defense = Rc::clone(&spell_book.components);
    println!("For defense, Ferris prepares: {:?}", spell_components_for_defense);

    let magic_circle = MagicCircle {
        energies: Arc::new(vec!["Mystic".to_string(), "Solar".to_string(), "Lunar".to_string()]),
    };

    let circle_for_summoning = Arc::clone(&magic_circle.energies);
    let circle_for_protection = Arc::clone(&magic_circle.energies);

    thread::spawn(move || {
        println!("Summoning circle energies: {:?}", circle_for_summoning);
    }).join().unwrap();

    thread::spawn(move || {
        println!("Protection circle energies: {:?}", circle_for_protection);
    }).join().unwrap();

    let binding = ["Ancient".to_string(), "Guardian".to_string()];
    let mut cow: Cow<[String]> = Cow::from(&binding[..]);
    if let Some(first) = cow.to_mut().first_mut() {
        *first = "Arcane".to_string();
    }
    println!("With a flick of his wand, Ferris modifies the spell chants from {:?} to {:?}", &["Ancient", "Guardian"], cow);

    println!("\nYour journey continues into the dark forest.🌲🌲🌲");
}
```

