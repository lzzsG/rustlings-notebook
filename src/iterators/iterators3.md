# Exercise 74

- Name: ```iterators3```
- Path: ```exercises/iterators/iterators3.rs```
#### Hint: 

The divide function needs to return the correct error when even division is not
possible.

The division_results variable needs to be collected into a collection type.

The result_with_list function needs to return a single Result where the success
case is a vector of integers and the failure case is a DivisionError.

The list_of_results function needs to return a vector of results.

See https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.collect for how
the `FromIterator` trait is used in `collect()`. This trait is REALLY powerful! It
can make the solution to this exercise infinitely easier.


---



