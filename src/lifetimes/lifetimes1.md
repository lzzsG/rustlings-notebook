# Exercise 65

- Name: ```lifetimes1```
- Path: ```exercises/lifetimes/lifetimes1.rs```
#### Hint: 

Let the compiler guide you. Also take a look at the book if you need help:
https://doc.rust-lang.org/book/ch10-03-lifetime-syntax.html


---



```rust,editable
// lifetimes1.rs
//
// The Rust compiler needs to know how to check whether supplied references are
// valid, so that it can let the programmer know if a reference is at risk of
// going out of scope before it is used. Remember, references are borrows and do
// not own their own data. What if their owner goes out of scope?
//
// Execute `rustlings hint lifetimes1` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

fn longest(x: &str, y: &str) -> &str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}

fn main() {
    let string1 = String::from("abcd");
    let string2 = "xyz";

    let result = longest(string1.as_str(), string2);
    println!("The longest string is '{}'", result);
}

```
