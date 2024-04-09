# Exercise 86

- Name: ```macros3```
- Path: ```exercises/macros/macros3.rs```
#### Hint: 

In order to use a macro outside of its module, you need to do something
special to the module to lift the macro out into its parent.

The same trick also works on "extern crate" statements for crates that have
exported macros, if you've seen any of those around.


---



```rust,editable
// macros3.rs
//
// Make me compile, without taking the macro out of the module!
//
// Execute `rustlings hint macros3` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

mod macros {
    macro_rules! my_macro {
        () => {
            println!("Check out my macro!");
        };
    }
}

fn main() {
    my_macro!();
}

```
