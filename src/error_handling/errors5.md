# Exercise 55

- Name: ```errors5```
- Path: ```exercises/error_handling/errors5.rs```
#### Hint: 

There are two different possible `Result` types produced within `main()`, which are
propagated using `?` operators. How do we declare a return type from `main()` that allows both?

Under the hood, the `?` operator calls `From::from` on the error value to convert it to a boxed
trait object, a `Box<dyn error::Error>`. This boxed trait object is polymorphic, and since all
errors implement the `error::Error` trait, we can capture lots of different errors in one "Box"
object.

Check out this section of the book:
https://doc.rust-lang.org/book/ch09-02-recoverable-errors-with-result.html#a-shortcut-for-propagating-errors-the--operator

Read more about boxing errors:
https://doc.rust-lang.org/stable/rust-by-example/error/multiple_error_types/boxing_errors.html

Read more about using the `?` operator with boxed errors:
https://doc.rust-lang.org/stable/rust-by-example/error/multiple_error_types/reenter_question_mark.html



---



