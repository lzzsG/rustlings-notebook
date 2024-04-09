# Exercise 54

- Name: ```errors4```
- Path: ```exercises/error_handling/errors4.rs```
#### Hint: 

`PositiveNonzeroInteger::new` is always creating a new instance and returning an `Ok` result.
It should be doing some checking, returning an `Err` result if those checks fail, and only
returning an `Ok` result if those checks determine that everything is... okay :)


---



```rust,editable
// errors4.rs
//
// Execute `rustlings hint errors4` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

#[derive(PartialEq, Debug)]
struct PositiveNonzeroInteger(u64);

#[derive(PartialEq, Debug)]
enum CreationError {
    Negative,
    Zero,
}

impl PositiveNonzeroInteger {
    fn new(value: i64) -> Result<PositiveNonzeroInteger, CreationError> {
        // Hmm...? Why is this only returning an Ok value?
        Ok(PositiveNonzeroInteger(value as u64))
    }
}

#[test]
fn test_creation() {
    assert!(PositiveNonzeroInteger::new(10).is_ok());
    assert_eq!(
        Err(CreationError::Negative),
        PositiveNonzeroInteger::new(-10)
    );
    assert_eq!(Err(CreationError::Zero), PositiveNonzeroInteger::new(0));
}

```
