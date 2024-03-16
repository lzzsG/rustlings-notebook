# Exercise 82

- Name: ```threads2```
- Path: ```exercises/threads/threads2.rs```
#### Hint: 

`Arc` is an Atomic Reference Counted pointer that allows safe, shared access
to **immutable** data. But we want to *change* the number of `jobs_completed`
so we'll need to also use another type that will only allow one thread to
mutate the data at a time. Take a look at this section of the book:
https://doc.rust-lang.org/book/ch16-03-shared-state.html#atomic-reference-counting-with-arct
and keep reading if you'd like more hints :)


Do you now have an `Arc` `Mutex` `JobStatus` at the beginning of main? Like:
`let status = Arc::new(Mutex::new(JobStatus { jobs_completed: 0 }));`
Similar to the code in the example in the book that happens after the text
that says "We can use Arc<T> to fix this.". If not, give that a try! If you
do and would like more hints, keep reading!!


Make sure neither of your threads are holding onto the lock of the mutex
while they are sleeping, since this will prevent the other thread from
being allowed to get the lock. Locks are automatically released when
they go out of scope.

If you've learned from the sample solutions, I encourage you to come
back to this exercise and try it again in a few days to reinforce
what you've learned :)


---



