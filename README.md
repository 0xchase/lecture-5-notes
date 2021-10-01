### Defining and enum

```rust
enum IpAddrKind {
    V4,
    V6,
}
```

We can create instances of each of the two variants of IpAddrKind like this, using the double colan to select some specific variant of our enum definition:

```rust
let four = IpAddrKind::V4;
let six = IpAddrKind::V6;
```

We can also give our enums a field. This new definition of the IpAddr enum says that both V4 and V6 variants will have associated String values.
### Enum examples

```rust
enum IpAddr {
    V4(String),
    V6(String),
}

let home = IpAddr::V4(String::from("127.0.0.1"));

let loopback = IpAddr::V6(String::from("::1"));
```

Or multiple fields.

```rust
enum IpAddr {
    V4(u8, u8, u8, u8),
    V6(String),
}

let home = IpAddr::V4(127, 0, 0, 1);

let loopback = IpAddr::V6(String::from("::1"));
```

In addition, just as you can define methods for structs, we can also do this for enums.

```
impl IpAddrKind {
    pub fn new() -> Self {
        return IpAddrKind::V4;
    }

}
```

---

### Option example

Here's some basic usage of the option type. 

```rust
let mut some_number = Some(5);
let mut some_string = Some("a string");

some_number = None;
some_string = None;

let mut absent_number: Option<i32> = None
absent_number = Some(5);
```

You cannot simply use an `Option<T>` type the same way you would type T. The following example, where an `Option<i8>` is added to an i8 won't compile. 

```rust
let x: i8 = 5;
let y: Option<i8> = Some(5);

let sum = x + y;
```

With the error:

```rust
$ cargo run
   Compiling enums v0.1.0 (file:///projects/enums)
error[E0277]: cannot add `Option<i8>` to `i8`
 --> src/main.rs:5:17
  |
5 |     let sum = x + y;
  |                 ^ no implementation for `i8 + Option<i8>`
  |
  = help: the trait `Add<Option<i8>>` is not implemented for `i8`

error: aborting due to previous error

For more information about this error, try `rustc --explain E0277`.
error: could not compile `enums`

To learn more, run the command again with --verbose.
```

In order to add these values you have to convert an `Option<T>` to a T. Generally, this helps catch one of the most common issues with null: assuming that something isn’t null when it actually is.

