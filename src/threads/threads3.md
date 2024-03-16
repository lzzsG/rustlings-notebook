# Exercise 83

- Name: ```threads3```
- Path: ```exercises/threads/threads3.rs```
#### Hint: 

An alternate way to handle concurrency between threads is to use
a mpsc (multiple producer, single consumer) channel to communicate.
With both a sending end and a receiving end, it's possible to
send values in one thread and receive them in another.
Multiple producers are possible by using clone() to create a duplicate
of the original sending end.
See https://doc.rust-lang.org/book/ch16-02-message-passing.html for more info.



---



