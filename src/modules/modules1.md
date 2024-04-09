# Exercise 41

- Name: ```modules1```
- Path: ```exercises/modules/modules1.rs```
#### Hint: 

Everything is private in Rust by default-- but there's a keyword we can use
to make something public! The compiler error should point to the thing that
needs to be public.


---



```rust,editable
// modules1.rs
//
// Execute `rustlings hint modules1` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

mod sausage_factory {
    // Don't let anybody outside of this module see this!
    fn get_secret_recipe() -> String {
        String::from("Ginger")
    }

    fn make_sausage() {
        get_secret_recipe();
        println!("sausage!");
    }
}

fn main() {
    sausage_factory::make_sausage();
}

```
