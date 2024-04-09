# Exercise 84

- Name: ```macros1```
- Path: ```exercises/macros/macros1.rs```
#### Hint: 

When you call a macro, you need to add something special compared to a
regular function call. If you're stuck, take a look at what's inside
`my_macro`.


---



```rust,editable
// macros1.rs
//
// Execute `rustlings hint macros1` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

macro_rules! my_macro {
    () => {
        println!("Check out my macro!");
    };
}

fn main() {
    my_macro();
}

```
