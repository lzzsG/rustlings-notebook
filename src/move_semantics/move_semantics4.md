# Exercise 28

- Name: ```move_semantics4```
- Path: ```exercises/move_semantics/move_semantics4.rs```
#### Hint: 

Stop reading whenever you feel like you have enough direction :) Or try
doing one step and then fixing the compiler errors that result!
So the end goal is to:
   - get rid of the first line in main that creates the new vector
   - so then `vec0` doesn't exist, so we can't pass it to `fill_vec`
   - `fill_vec` has had its signature changed, which our call should reflect
   - since we're not creating a new vec in `main` anymore, we need to create
     a new vec in `fill_vec`, similarly to the way we did in `main`


---



