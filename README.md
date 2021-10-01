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

