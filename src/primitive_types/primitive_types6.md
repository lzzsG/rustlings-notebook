# Exercise 22

- Name: ```primitive_types6```
- Path: ```exercises/primitive_types/primitive_types6.rs```
#### Hint: 

While you could use a destructuring `let` for the tuple here, try
indexing into it instead, as explained in the last example of the
Data Types -> The Tuple Type section of the book:
https://doc.rust-lang.org/book/ch03-02-data-types.html#the-tuple-type
Now you have another tool in your toolbox!


---



```rust,editable
// primitive_types6.rs
//
// Use a tuple index to access the second element of `numbers`. You can put the
// expression for the second element where ??? is so that the test passes.
//
// Execute `rustlings hint primitive_types6` or use the `hint` watch subcommand
// for a hint.

// I AM NOT DONE

#[test]
fn indexing_tuple() {
    let numbers = (1, 2, 3);
    // Replace below ??? with the tuple indexing syntax.
    let second = ???;

    assert_eq!(2, second,
        "This is not the 2nd number in the tuple!")
}

```
