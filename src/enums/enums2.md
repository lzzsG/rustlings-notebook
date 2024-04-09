# Exercise 35

- Name: ```enums2```
- Path: ```exercises/enums/enums2.rs```
#### Hint: 

You can create enumerations that have different variants with different types
such as no data, anonymous structs, a single string, tuples, ...etc


---



```rust,editable
// enums2.rs
//
// Execute `rustlings hint enums2` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

#[derive(Debug)]
enum Message {
    // TODO: define the different variants used below
}

impl Message {
    fn call(&self) {
        println!("{:?}", self);
    }
}

fn main() {
    let messages = [
        Message::Move { x: 10, y: 30 },
        Message::Echo(String::from("hello world")),
        Message::ChangeColor(200, 255, 255),
        Message::Quit,
    ];

    for message in &messages {
        message.call();
    }
}

```
