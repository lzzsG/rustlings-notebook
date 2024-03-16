# Exercise 30

- Name: ```move_semantics6```
- Path: ```exercises/move_semantics/move_semantics6.rs```
#### Hint: 

To find the answer, you can consult the book section "References and Borrowing":
https://doc.rust-lang.org/stable/book/ch04-02-references-and-borrowing.html
The first problem is that `get_char` is taking ownership of the string.
So `data` is moved and can't be used for `string_uppercase`
`data` is moved to `get_char` first, meaning that `string_uppercase` cannot manipulate the data.
Once you've fixed that, `string_uppercase`'s function signature will also need to be adjusted.
Can you figure out how?

Another hint: it has to do with the `&` character.


---



