# Exercise 67

- Name: ```lifetimes3```
- Path: ```exercises/lifetimes/lifetimes3.rs```
#### Hint: 

If you use a lifetime annotation in a struct's fields, where else does it need to be added?


---



```rust,editable
// lifetimes3.rs
//
// Lifetimes are also needed when structs hold references.
//
// Execute `rustlings hint lifetimes3` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

struct Book {
    author: &str,
    title: &str,
}

fn main() {
    let name = String::from("Jill Smith");
    let title = String::from("Fish Flying");
    let book = Book { author: &name, title: &title };

    println!("{} by {}", book.title, book.author);
}

```
