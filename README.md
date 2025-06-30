# AAU

> a rust-like language

## Example

```rust
// example.aau
#[start]
fn main() {
    let number = box 1; // let number: i32.Box = box 1;
	println!("{*number}"); // output `1` and newline
}
```

equivalent to

```rust
// example.rs
fn main() {
    // let number = box 1; // nightly
    let number = Box::new(1); // stable
    println!("{}", *number)
}
```

## Docs

[see the docs](./docs/index.md)