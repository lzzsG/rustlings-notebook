# Exercise 51

- Name: ```errors1```
- Path: ```exercises/error_handling/errors1.rs```
#### Hint: 

`Ok` and `Err` are one of the variants of `Result`, so what the tests are saying
is that `generate_nametag_text` should return a `Result` instead of an
`Option`.

To make this change, you'll need to:
   - update the return type in the function signature to be a Result<String, String> that
     could be the variants `Ok(String)` and `Err(String)`
   - change the body of the function to return `Ok(stuff)` where it currently
     returns `Some(stuff)`
   - change the body of the function to return `Err(error message)` where it
     currently returns `None`


---



