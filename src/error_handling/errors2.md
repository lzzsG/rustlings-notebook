# Exercise 52

- Name: ```errors2```
- Path: ```exercises/error_handling/errors2.rs```
#### Hint: 

One way to handle this is using a `match` statement on
`item_quantity.parse::<i32>()` where the cases are `Ok(something)` and
`Err(something)`. This pattern is very common in Rust, though, so there's
a `?` operator that does pretty much what you would make that match statement
do for you! Take a look at this section of the Error Handling chapter:
https://doc.rust-lang.org/book/ch09-02-recoverable-errors-with-result.html#a-shortcut-for-propagating-errors-the--operator
and give it a try!


---



