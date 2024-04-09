# Exercise 85

- Name: ```macros2```
- Path: ```exercises/macros/macros2.rs```
#### Hint: 

Macros don't quite play by the same rules as the rest of Rust, in terms of
what's available where.

Unlike other things in Rust, the order of "where you define a macro" versus
"where you use it" actually matters.


---



```rust,editable
// macros2.rs
//
// Execute `rustlings hint macros2` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

fn main() {
    my_macro!();
}

macro_rules! my_macro {
    () => {
        println!("Check out my macro!");
    };
}

```
