# Exercise 26

- Name: ```move_semantics2```
- Path: ```exercises/move_semantics/move_semantics2.rs```
#### Hint: 

When running this exercise for the first time, you'll notice an error about
"borrow of moved value". In Rust, when an argument is passed to a function and
it's not explicitly returned, you can't use the original variable anymore.
We call this "moving" a variable. When we pass `vec0` into `fill_vec`, it's being
"moved" into `vec1`, meaning we can't access `vec0` anymore after the fact.
Rust provides a couple of different ways to mitigate this issue, feel free to try them all:
1. You could make another, separate version of the data that's in `vec0` and pass that
   to `fill_vec` instead.
2. Make `fill_vec` borrow its argument instead of taking ownership of it,
   and then copy the data within the function (`vec.clone()`) in order to return an owned
   `Vec<i32>`.
3. Or, you could make `fill_vec` *mutably* borrow a reference to its argument (which will need to be
   mutable), modify it directly, then not return anything. This means that `vec0` will change over the
   course of the function, and makes `vec1` redundant (make sure to change the parameters of the `println!`
   statements if you go this route)



---



```rust,editable
// move_semantics2.rs
//
// Expected output:
// vec0 has length 3, with contents `[22, 44, 66]`
// vec1 has length 4, with contents `[22, 44, 66, 88]`
//
// Execute `rustlings hint move_semantics2` or use the `hint` watch subcommand
// for a hint.

// I AM NOT DONE

fn main() {
    let vec0 = Vec::new();

    let mut vec1 = fill_vec(vec0);

    println!("{} has length {}, with contents: `{:?}`", "vec0", vec0.len(), vec0);

    vec1.push(88);

    println!("{} has length {}, with contents `{:?}`", "vec1", vec1.len(), vec1);
}

fn fill_vec(vec: Vec<i32>) -> Vec<i32> {
    let mut vec = vec;

    vec.push(22);
    vec.push(44);
    vec.push(66);

    vec
}

```
