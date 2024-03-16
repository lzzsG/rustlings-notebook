# Exercise 77

- Name: ```box1```
- Path: ```exercises/smart_pointers/box1.rs```
#### Hint: 

Step 1
The compiler's message should help: since we cannot store the value of the actual type
when working with recursive types, we need to store a reference (pointer) to its value.
We should, therefore, place our `List` inside a `Box`. More details in the book here:
https://doc.rust-lang.org/book/ch15-01-box.html#enabling-recursive-types-with-boxes

Step 2
Creating an empty list should be fairly straightforward (hint: peek at the assertions).
For a non-empty list keep in mind that we want to use our Cons "list builder".
Although the current list is one of integers (i32), feel free to change the definition
and try other types!



---



