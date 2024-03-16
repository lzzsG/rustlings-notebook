# Exercise 73

- Name: ```iterators2```
- Path: ```exercises/iterators/iterators2.rs```
#### Hint: 

Step 1
The variable `first` is a `char`. It needs to be capitalized and added to the
remaining characters in `c` in order to return the correct `String`.
The remaining characters in `c` can be viewed as a string slice using the
`as_str` method.
The documentation for `char` contains many useful methods.
https://doc.rust-lang.org/std/primitive.char.html

Step 2
Create an iterator from the slice. Transform the iterated values by applying
the `capitalize_first` function. Remember to collect the iterator.

Step 3.
This is surprisingly similar to the previous solution. Collect is very powerful
and very general. Rust just needs to know the desired type.


---



