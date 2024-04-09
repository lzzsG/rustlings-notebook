# Exercise 66

- Name: ```lifetimes2```
- Path: ```exercises/lifetimes/lifetimes2.rs```
#### Hint: 

Remember that the generic lifetime 'a will get the concrete lifetime that is equal to the smaller of the lifetimes of x and y.
You can take at least two paths to achieve the desired result while keeping the inner block:
1. Move the string2 declaration to make it live as long as string1 (how is result declared?)
2. Move println! into the inner block


---



```rust,editable
// lifetimes2.rs
//
// So if the compiler is just validating the references passed to the annotated
// parameters and the return type, what do we need to change?
//
// Execute `rustlings hint lifetimes2` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}

fn main() {
    let string1 = String::from("long string is long");
    let result;
    {
        let string2 = String::from("xyz");
        result = longest(string1.as_str(), string2.as_str());
    }
    println!("The longest string is '{}'", result);
}

```
