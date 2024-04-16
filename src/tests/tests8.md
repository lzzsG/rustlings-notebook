# Exercise 99

- Name: ```tests8```
- Path: ```exercises/tests/tests8.rs```
#### Hint: 

The command to set up an environment variable is "rustc-cfg=CFG[="VALUE"]", while
the square brackets means optional. Be sure what `CFG` and `VALUE` you want here.


---



```rust,editable
// tests8.rs
//
// This execrise shares `build.rs` with the previous exercise.
// You need to add some code to `build.rs` to make both this exercise and
// the previous one work.
//
// Execute `rustlings hint tests8` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

fn main() {}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_success() {
        #[cfg(feature = "pass")]
        return;

        panic!("no cfg set");
    }
}

```

---




---

## 扩展知识点与解答：
