### String vs &str
|str|String|
|-|-|
|immutable|mutable|
|fixed|dynamic|
|pointer|struct|

  

[rust docs | Book](https://doc.rust-lang.org/book/ch04-02-references-and-borrowing.html)

---
#### References and Borrowing
- If you have a mutable reference to a value, you can have no other references to that value
- Cannot have a mutable reference while we have an immutable one to the same value.
- Multiple immutable references are allowed
- A _data race_ is similar to a race condition and happens when these three behaviours occur:
	- Two or more pointers access the same data at the same time.
	-   At least one of the pointers is being used to write to the data.
	-   There’s no mechanism being used to synchronize access to the data.
- The ability of the compiler to tell that a reference is no longer being used at a point before the end of the scope is called _Non-Lexical Lifetimes_

#### String slices
- A _string slice_ is a reference to part of a `String`
- Slice data structure stores the starting position and the length of the slice
- We can pass a slice of the `String` or a reference to the `String` to `&str` parameter. This flexibility takes advantage of _deref coercions_

```rust
let s = String::from("hello world");
let s2: &String = &s; // not a string slice, reference to String
```








