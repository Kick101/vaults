### String vs &str
|str|String|
|-|-|
|immutable|mutable|
|fixed|dynamic|
|pointer|struct|

___Error Code___
```rust
let test1: &str = "test1";  // Borrow, like pointer
let test2: String = "test2".to_string();  // Ownership moved to test2
println!("{} {}",test1,test2);  
let test3 = test2;  // Ownership moved to test3, test2 no  longer valid
println!("{:?}",(test1,test2,test3)); // Error!!
```
___Legit Code___
```rust
let test1: &str = "test1";  // Borrow
let test2: &String = &"test2".to_string();  // Only borrow happened
println!("{} {}",test1,test2);  
let test3 = test2;  // test3 borrowed from test2
println!("{:?}",(test1,test2,test3));
```

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







