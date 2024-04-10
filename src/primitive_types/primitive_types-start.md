# primitive_types

---

在即将开始的一系列练习中，我们将深入探索Rust中的基本数据类型，这些类型构成了Rust语言的基石。从布尔值到字符，从数组到元组，每一种类型都承载着特定的数据和操作方式，理解它们对于掌握Rust编程至关重要。

1. **布尔类型（`bool`）**：我们将从最基本的布尔类型开始，这是每种编程语言中都不可或缺的一部分。你将学习如何声明和使用布尔变量来控制程序的流程。

2. **字符类型（`char`）**：接下来，我们将探索Rust中的字符类型。与其他语言相比，Rust的`char`类型是32位的，能够表示任何Unicode字符，这包括了从ASCII字符到表情符号等。你将尝试使用不同种类的字符，并学会如何检查它们的属性。

3. **数组**：数组是存储一系列相同类型元素的集合。我们将练习创建一个拥有至少100个元素的数组，这将帮助你熟悉数组的声明和初始化方法，以及如何高效地处理大量数据。

4. **切片**：切片是Rust中一个非常强大的特性，它允许你引用数组或向量的一部分而不拥有它。通过这个练习，你将学习如何从一个数组中获取切片，并理解切片如何提供对数据的灵活访问。

5. **元组**：元组是可以包含多种类型元素的复合类型。我们将通过两个练习深入元组，首先学习如何解构元组，提取元组中的值。紧接着，我们将探索如何通过索引直接访问元组中的元素，这两种技能都对于处理复杂数据结构非常有用。

通过这些练习，你将逐步建立起对Rust基本数据类型的深入理解，并学会如何在实际编程中灵活运用这些类型。每一步都为你揭开Rust编程世界的更多可能，让我们一起开始这段探索之旅吧！

---

```rust
// 你不必现在理解以下代码，不过你可以尝试运行它。

fn main() {
    let has_magic_map: bool = true;

    let symbol_of_ancient: char = '🗺';

    let magic_numbers: [i32; 100] = [10; 100]; 

    let magic_slice = &magic_numbers[0..3]; 

    let artifact_location: (f64, f64, char) = (040.002, 116.320, 'x');

    if has_magic_map {
        println!("You gain the treasure chest, with glowing {} symbols glittering on it, in which is\na magic map", symbol_of_ancient);
    }
    
	let mut magic_vec = magic_slice.to_vec();
	magic_vec.insert(0, 100);
	let new_magic_slice = &magic_vec[..];
    println!("The map reveals a sequence of magic numbers:");
    for &number in new_magic_slice.iter() {
        print!("{}", number);
    }
    println!("000010011111101101011100000\nThese numbers seem to be guiding you.");

    let (x, y, marker) = artifact_location;
    println!("The ancient artifact is located at ({}, {}) marked with a '{}'.", x, y, marker);

    println!("Double-checking, the artifact indeed lies at latitude {} and longitude {}.", artifact_location.0, artifact_location.1);

    println!("\n🌲🌲🌲Your journey continues into the dark forest.🌲🌲🌲\nAbove, a break in the trees reveals a sky full of stars, lighting your way with \ncelestial clarity.");
}

```

