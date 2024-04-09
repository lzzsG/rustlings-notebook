# Exercise 89

- Name: ```clippy2```
- Path: ```exercises/clippy/clippy2.rs```
#### Hint: 

`for` loops over Option values are more clearly expressed as an `if let`


---



```rust,editable
// clippy2.rs
// 
// Execute `rustlings hint clippy2` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

fn main() {
    let mut res = 42;
    let option = Some(12);
    for x in option {
        res += x;
    }
    println!("{}", res);
}

```
