# Exercise 72

- Name: ```iterators1```
- Path: ```exercises/iterators/iterators1.rs```
#### Hint: 

Step 1:
We need to apply something to the collection `my_fav_fruits` before we start to go through
it. What could that be? Take a look at the struct definition for a vector for inspiration:
https://doc.rust-lang.org/std/vec/struct.Vec.html
Step 2 & step 3:
Very similar to the lines above and below. You've got this!
Step 4:
An iterator goes through all elements in a collection, but what if we've run out of
elements? What should we expect here? If you're stuck, take a look at
https://doc.rust-lang.org/std/iter/trait.Iterator.html for some ideas.



---



```rust,editable
// iterators1.rs
//
// When performing operations on elements within a collection, iterators are
// essential. This module helps you get familiar with the structure of using an
// iterator and how to go through elements within an iterable collection.
//
// Make me compile by filling in the `???`s
//
// Execute `rustlings hint iterators1` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

fn main() {
    let my_fav_fruits = vec!["banana", "custard apple", "avocado", "peach", "raspberry"];

    let mut my_iterable_fav_fruits = ???;   // TODO: Step 1

    assert_eq!(my_iterable_fav_fruits.next(), Some(&"banana"));
    assert_eq!(my_iterable_fav_fruits.next(), ???);     // TODO: Step 2
    assert_eq!(my_iterable_fav_fruits.next(), Some(&"avocado"));
    assert_eq!(my_iterable_fav_fruits.next(), ???);     // TODO: Step 3
    assert_eq!(my_iterable_fav_fruits.next(), Some(&"raspberry"));
    assert_eq!(my_iterable_fav_fruits.next(), ???);     // TODO: Step 4
}

```
