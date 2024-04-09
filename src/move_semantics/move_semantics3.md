# Exercise 27

- Name: ```move_semantics3```
- Path: ```exercises/move_semantics/move_semantics3.rs```
#### Hint: 

The difference between this one and the previous ones is that the first line
of `fn fill_vec` that had `let mut vec = vec;` is no longer there. You can,
instead of adding that line back, add `mut` in one place that will change
an existing binding to be a mutable binding instead of an immutable one :)


---



```rust,editable
// move_semantics3.rs
//
// Make me compile without adding new lines-- just changing existing lines! (no
// lines with multiple semicolons necessary!)
//
// Execute `rustlings hint move_semantics3` or use the `hint` watch subcommand
// for a hint.

// I AM NOT DONE

fn main() {
    let vec0 = Vec::new();

    let mut vec1 = fill_vec(vec0);

    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);

    vec1.push(88);

    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);
}

fn fill_vec(vec: Vec<i32>) -> Vec<i32> {
    vec.push(22);
    vec.push(44);
    vec.push(66);

    vec
}

```
