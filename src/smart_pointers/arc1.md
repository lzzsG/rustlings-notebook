# Exercise 79

- Name: ```arc1```
- Path: ```exercises/smart_pointers/arc1.rs```
#### Hint: 

Make `shared_numbers` be an `Arc` from the numbers vector. Then, in order
to avoid creating a copy of `numbers`, you'll need to create `child_numbers`
inside the loop but still in the main thread.

`child_numbers` should be a clone of the Arc of the numbers instead of a
thread-local copy of the numbers.

This is a simple exercise if you understand the underlying concepts, but if this
is too much of a struggle, consider reading through all of Chapter 16 in the book:
https://doc.rust-lang.org/stable/book/ch16-00-concurrency.html



---



