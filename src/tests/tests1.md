# Exercise 68

- Name: ```tests1```
- Path: ```exercises/tests/tests1.rs```
#### Hint: 

You don't even need to write any code to test -- you can just test values and run that, even
though you wouldn't do that in real life :) `assert!` is a macro that needs an argument.
Depending on the value of the argument, `assert!` will do nothing (in which case the test will
pass) or `assert!` will panic (in which case the test will fail). So try giving different values
to `assert!` and see which ones compile, which ones pass, and which ones fail :)


---



```rust,editable
// tests1.rs
//
// Tests are important to ensure that your code does what you think it should
// do. Tests can be run on this file with the following command: rustlings run
// tests1
//
// This test has a problem with it -- make the test compile! Make the test pass!
// Make the test fail!
//
// Execute `rustlings hint tests1` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

#[cfg(test)]
mod tests {
    #[test]
    fn you_can_assert() {
        assert!();
    }
}

```
