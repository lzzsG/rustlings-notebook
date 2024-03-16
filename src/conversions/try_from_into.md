# Exercise 94

- Name: ```try_from_into```
- Path: ```exercises/conversions/try_from_into.rs```
#### Hint: 

Follow the steps provided right before the `TryFrom` implementation.
You can also use the example at https://doc.rust-lang.org/std/convert/trait.TryFrom.html

Is there an implementation of `TryFrom` in the standard library that
can both do the required integer conversion and check the range of the input?

Another hint: Look at the test cases to see which error variants to return.

Yet another hint: You can use the `map_err` or `or` methods of `Result` to
convert errors.

Yet another hint: If you would like to propagate errors by using the `?`
operator in your solution, you might want to look at
https://doc.rust-lang.org/stable/rust-by-example/error/multiple_error_types/reenter_question_mark.html

Challenge: Can you make the `TryFrom` implementations generic over many integer types?


---



