# Exercise 60

- Name: ```traits2```
- Path: ```exercises/traits/traits2.rs```
#### Hint: 

Notice how the trait takes ownership of 'self',and returns `Self`.
Try mutating the incoming string vector. Have a look at the tests to see
what the result should look like!

Vectors provide suitable methods for adding an element at the end. See
the documentation at: https://doc.rust-lang.org/std/vec/struct.Vec.html


---



```rust,editable
// traits2.rs
//
// Your task is to implement the trait `AppendBar` for a vector of strings. To
// implement this trait, consider for a moment what it means to 'append "Bar"'
// to a vector of strings.
//
// No boiler plate code this time, you can do this!
//
// Execute `rustlings hint traits2` or use the `hint` watch subcommand for a hint.

// I AM NOT DONE

trait AppendBar {
    fn append_bar(self) -> Self;
}

// TODO: Implement trait `AppendBar` for a vector of strings.

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn is_vec_pop_eq_bar() {
        let mut foo = vec![String::from("Foo")].append_bar();
        assert_eq!(foo.pop().unwrap(), String::from("Bar"));
        assert_eq!(foo.pop().unwrap(), String::from("Foo"));
    }
}

```
