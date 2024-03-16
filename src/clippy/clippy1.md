# Exercise 88

- Name: ```clippy1```
- Path: ```exercises/clippy/clippy1.rs```
#### Hint: 

Rust stores the highest precision version of any long or infinite precision
mathematical constants in the Rust standard library.
https://doc.rust-lang.org/stable/std/f32/consts/index.html

We may be tempted to use our own approximations for certain mathematical constants,
but clippy recognizes those imprecise mathematical constants as a source of
potential error.
See the suggestions of the clippy warning in compile output and use the
appropriate replacement constant from std::f32::consts...


---



