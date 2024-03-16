# Exercise 56

- Name: ```errors6```
- Path: ```exercises/error_handling/errors6.rs```
#### Hint: 

This exercise uses a completed version of `PositiveNonzeroInteger` from
errors4.

Below the line that TODO asks you to change, there is an example of using
the `map_err()` method on a `Result` to transform one type of error into
another. Try using something similar on the `Result` from `parse()`. You
might use the `?` operator to return early from the function, or you might
use a `match` expression, or maybe there's another way!

You can create another function inside `impl ParsePosNonzeroError` to use
with `map_err()`.

Read more about `map_err()` in the `std::result` documentation:
https://doc.rust-lang.org/std/result/enum.Result.html#method.map_err


---



