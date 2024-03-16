# Exercise 93

- Name: ```from_str```
- Path: ```exercises/conversions/from_str.rs```
#### Hint: 

The implementation of FromStr should return an Ok with a Person object,
or an Err with an error if the string is not valid.

This is almost like the `from_into` exercise, but returning errors instead
of falling back to a default value.

Look at the test cases to see which error variants to return.

Another hint: You can use the `map_err` method of `Result` with a function
or a closure to wrap the error from `parse::<usize>`.

Yet another hint: If you would like to propagate errors by using the `?`
operator in your solution, you might want to look at
https://doc.rust-lang.org/stable/rust-by-example/error/multiple_error_types/reenter_question_mark.html



---



