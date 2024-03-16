# Exercise 78

- Name: ```rc1```
- Path: ```exercises/smart_pointers/rc1.rs```
#### Hint: 

This is a straightforward exercise to use the Rc<T> type. Each Planet has
ownership of the Sun, and uses Rc::clone() to increment the reference count of the Sun.
After using drop() to move the Planets out of scope individually, the reference count goes down.
In the end the sun only has one reference again, to itself. See more at:
https://doc.rust-lang.org/book/ch15-04-rc.html

* Unfortunately Pluto is no longer considered a planet :(



---



