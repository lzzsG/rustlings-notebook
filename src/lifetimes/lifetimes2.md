# Exercise 66

- Name: ```lifetimes2```
- Path: ```exercises/lifetimes/lifetimes2.rs```
#### Hint: 

Remember that the generic lifetime 'a will get the concrete lifetime that is equal to the smaller of the lifetimes of x and y.
You can take at least two paths to achieve the desired result while keeping the inner block:
1. Move the string2 declaration to make it live as long as string1 (how is result declared?)
2. Move println! into the inner block


---



