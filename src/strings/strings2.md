# Exercise 38

- Name: ```strings2```
- Path: ```exercises/strings/strings2.rs```
#### Hint: 

Yes, it would be really easy to fix this by just changing the value bound to `word` to be a
string slice instead of a `String`, wouldn't it?? There is a way to add one character to line
12, though, that will coerce the `String` into a string slice.

Side note: If you're interested in learning about how this kind of reference conversion works, you can jump ahead in the book and read this part in the smart pointers chapter: https://doc.rust-lang.org/stable/book/ch15-02-deref.html#implicit-deref-coercions-with-functions-and-methods


---



```rust,editable
// strings2.rs
//
// Make me compile without changing the function signature!
//
// Execute `rustlings hint strings2` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

fn main() {
    let word = String::from("green"); // Try not changing this line :)
    if is_a_color_word(word) {
        println!("That is a color word I know!");
    } else {
        println!("That is not a color word I know.");
    }
}

fn is_a_color_word(attempt: &str) -> bool {
    attempt == "green" || attempt == "blue" || attempt == "red"
}

```
