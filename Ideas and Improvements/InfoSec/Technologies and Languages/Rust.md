### String vs &str
|str|String|
|-|-|
|immutable|mutable|
|fixed|dynamic|
|pointer|struct|

___Error Code___
```rust
let test1: &str = "test1";  // Borrow, like pointer
let test2: String = "test2".to_string();  // Ownership moved to test2, like i32
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