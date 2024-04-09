# Exercise 50

- Name: ```options3```
- Path: ```exercises/options/options3.rs```
#### Hint: 

The compiler says a partial move happened in the `match`
statement. How can this be avoided? The compiler shows the correction
needed. After making the correction as suggested by the compiler, do
read: https://doc.rust-lang.org/std/keyword.ref.html


---



```rust,editable
// options3.rs
//
// Execute `rustlings hint options3` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let y: Option<Point> = Some(Point { x: 100, y: 200 });

    match y {
        Some(p) => println!("Co-ordinates are {},{} ", p.x, p.y),
        _ => panic!("no match!"),
    }
    y; // Fix without deleting this line.
}

```
