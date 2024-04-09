# Exercise 25

- Name: ```move_semantics1```
- Path: ```exercises/move_semantics/move_semantics1.rs```
#### Hint: 

So you've got the "cannot borrow immutable local variable `vec1` as mutable" error on line 13,
right? The fix for this is going to be adding one keyword, and the addition is NOT on line 13
where the error is.

Also: Try accessing `vec0` after having called `fill_vec()`. See what happens!


---



```rust,editable
// move_semantics1.rs
//
// Execute `rustlings hint move_semantics1` or use the `hint` watch subcommand
// for a hint.

// I AM NOT DONE

fn main() {
    let vec0 = Vec::new();

    let vec1 = fill_vec(vec0);

    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);

    vec1.push(88);

    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);
}

fn fill_vec(vec: Vec<i32>) -> Vec<i32> {
    let mut vec = vec;

    vec.push(22);
    vec.push(44);
    vec.push(66);

    vec
}

```
