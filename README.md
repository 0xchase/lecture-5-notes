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

### Pattern example

*Open pattern.rs*

This example shows a basic match with the ipaddr enum we created previously. We can also match with specific values in the pattern match.

```rust
fn main() {
        enum IpAddr {
            V4(u8, u8, u8, u8),
            V6(String),
        }

        let home = IpAddr::V4(127, 0, 0, 1);
        let loopback = IpAddr::V6(String::from("::1"));

        match home {
                IpAddr::V4(a, b, c, d) => println!("Is V4"),
                IpAddr::V4(127, b, c, d) => println!("Is V4 loopback"), // Add this line
                IpAddr::V6(a) => println!("Is V6")
        };
}
```

Rust will require that our matches be *exaustive*. 

```rust
fn main() {
        enum IpAddr {
            V4(u8, u8, u8, u8),
            V6(String),
        }

        let home = IpAddr::V4(127, 0, 0, 1);
        let loopback = IpAddr::V6(String::from("::1"));

        match home {
                IpAddr::V4(a, b, c, d) => println!("Is V4"),
                IpAddr::V4(127, b, c, d) => println!("Is V4 loopback"),
                // Remove this line
        };
}
```

For example, if we don't attempt to match with the V6 variant, the compiler will throw an error. 

```rust
error[E0004]: non-exhaustive patterns: `V6(_)` not covered
  --> temp.rs:10:8
   |
2  | /     enum IpAddr {
3  | |         V4(u8, u8, u8, u8),
4  | |         V6(String),
   | |         -- not covered
5  | |     }
   | |_____- `main::IpAddr` defined here
...
10 |       match home {
   |             ^^^^ pattern `V6(_)` not covered
   |
   = help: ensure that all possible cases are being handled, possibly by adding wildcards or more match arms
   = note: the matched value is of type `main::IpAddr`
```

We don't have to match every pattern explicitly, however. We can use _ to catch all remaining cases. 

```rust
fn main() {
        enum IpAddr {
            V4(u8, u8, u8, u8),
            V6(String),
        }

        let home = IpAddr::V4(127, 0, 0, 1);
        let loopback = IpAddr::V6(String::from("::1"));

        match home {
                IpAddr::V4(a, b, c, d) => println!("Is V4"),
                IpAddr::V4(127, b, c, d) => println!("Is V4 loopback"),
                _ => println!("Is V6") // Convert this line
        };
}
```

Pattern matches can also be used to set variables as shown below. 

```rust
fn main() {
        enum IpAddr {
            V4(u8, u8, u8, u8),
            V6(String),
        }

        let home = IpAddr::V4(127, 0, 0, 1);
        let loopback = IpAddr::V6(String::from("::1"));

        let loopback = match home { // Add this line
                IpAddr::V4(127, b, c, d) => Some(home),
                _ => None
        };
}
```

### If let example

We want to do something with the `Some(3)` match but do nothing with any other `Some<u8>` value or the None value. To satisfy the match expression, we have to add _ => () after processing just one variant, which is a lot of boilerplate code to add.

*Write some match thing*

Instead, we could write this in a shorter way using if let.

```rust
fn main() {
        enum IpAddr {
            V4(u8, u8, u8, u8),
            V6(String),
        }

        let home = IpAddr::V4(127, 0, 0, 1);
        let loopback = IpAddr::V6(String::from("::1"));

        let loopback = match home { // Add this line
                IpAddr::V4(127, b, c, d) => Some(home),
                _ => None
        };

        if let Some(addr) = loopback {
            println!("Found loopback addr");
        } else {
            println!("No address found");
        }
}

```

### Error example


Let’s call a function that returns a Result value because the function could fail. 

```rust
use std::fs::File;

fn main() {
    let f = File::open("hello.txt");
}
```

How do we know what type File::open returns? We can find out by asking the compiler, or with online documentation.

```rust
    let f: u32 = File::open("hello.txt");
```

This will produce an error since the method returns a Result type. Since it's an enum, we can match with it as shown below.

```rust
use std::fs::File;

fn main() {
    let f = File::open("hello.txt");

    let f = match f {
        Ok(file) => file,
        Err(error) => panic!("Problem opening the file: {:?}", error),
    };
}
```

We can also nest our matching expressions to match with a specific kind of error.

The type of the value that File::open returns inside the Err variant is io::Error, which is a struct provided by the standard library. This struct has a method kind that we can call to get an io::ErrorKind value. The enum io::ErrorKind is provided by the standard library and has variants representing the different kinds of errors that might result from an io operation. The variant we want to use is ErrorKind::NotFound, which indicates the file we’re trying to open doesn’t exist yet. So we match on f, but we also have an inner match on error.kind().

```rust
use std::fs::File;
use std::io::ErrorKind;

fn main() {
    let f = File::open("hello.txt");

    let f = match f {
        Ok(file) => file,
        Err(error) => match error.kind() {
            ErrorKind::NotFound => match File::create("hello.txt") {
                Ok(fc) => fc,
                Err(e) => panic!("Problem creating the file: {:?}", e),
            },
            other_error => {
                panic!("Problem opening the file: {:?}", other_error)
            }
        },
    };
}
```

This is also a circumstance when a if-let match would be useful.

*Switch initial code to if-let. *

### Shortcut example

```rust
use std::fs::File;

fn main() {
    let f = File::open("hello.txt").unwrap();
}
```

If we run this code and the file doesn't exist, it will panic with the following error. 

```rust
thread 'main' panicked at 'called `Result::unwrap()` on an `Err` value: Error {
repr: Os { code: 2, message: "No such file or directory" } }',
src/libcore/result.rs:906:4
```

We can also use `expect()`, to print a specific message when our code runs. 

```rust
use std::fs::File;

fn main() {
    let f = File::open("hello.txt").expect("Failed to open hello.txt");
}
```

If we run this example we get

```
thread 'main' panicked at 'Failed to open hello.txt: Error { repr: Os { code:
2, message: "No such file or directory" } }', src/libcore/result.rs:906:4
```

### Panic example

We briefly above saw the `panic!` macro. This macro is used to generate an unrecoverable error. Here is an example. 

```rust
fn main() {
    panic!("crash and burn");
}
```

Your programs may also panic for reasons like accessing an out-of-bounds vector element.

```rust
fn main() {
    let v = vec![1, 2, 3];

    v[99];
}
```

### Propagation example


A naive way to accompolish this is shown below.

```rust
use std::fs::File;
use std::io;
use std::io::Read;

fn read_username_from_file() -> Result<String, io::Error> {
    let f = File::open("hello.txt");

    let mut f = match f {
        Ok(file) => file,
        Err(e) => return Err(e),
    };

    let mut s = String::new();

    match f.read_to_string(&mut s) {
        Ok(_) => Ok(s),
        Err(e) => Err(e),
    }
}
```

Using the match expressions here is quite verbose. To make this more concise, we can use the `?` operator, which makes the above example much more concise.

```rust
use std::fs::File;
use std::io;
use std::io::Read;

fn read_username_from_file() -> Result<String, io::Error> {
    let mut s = String::new();

    File::open("hello.txt")?.read_to_string(&mut s)?;

    Ok(s)
}
```

This operator should only be used on functions that return a result. If we attempt to use this in the main function, for example, the compiler will throw an error. 
