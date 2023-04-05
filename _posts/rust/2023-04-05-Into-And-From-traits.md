---
layout: post
title: Rust notes - Into and From traits
categories: rust
---

Notes on how to use Into and From traits.

## Implementing `From` trait 

When you want to convert from type 'A' to type 'B', one way we can do that is to implement
`std::convert::From` trait for 'B'. There is only one method in this trait that we must implement.

```rust
pub trait From<T> {
    fn from(T) -> Self;
}
```

The `from` method, takes in an instance of type 'A' and will return instance of type 'B'.

```rust
impl From<A> for B {
    fn from(t: A) -> Self {
        
        // Some implementation...

    }
}
```

To do the actual conversion, the `from` method can be called like so

```rust
let _: B = B::from (A);
```

or another way is to call `A::into` method. 

When we implement `From` trait, `Into` trait also gets implemented. So here we implemented `From` 
trait for 'B' explicitly, which implicitly implements `Into` trait for 'A'.

```rust
let _: B = A.into();        // this
let _: B = A::into(A);      // or this
```

## Example

Conversion of StandardChars to char.

This can be done using
1. char::from (<StandardChars instance>)
2. StandardChars::into (<StandardChars instance>)

```rust
enum StandardChars {
    A,
    B,
    C,
}

impl From<StandardChars> for char {
    fn from(t: StandardChars) -> Self {
        match t {
            StandardChars::A => 'A',
            StandardChars::B => 'B',
            StandardChars::C => 'C',
        }
    }
}

fn main() {
    let _: char = StandardChars::into(StandardChars::A);
    let _: char = StandardChars::A.into();
    let _: char = StandardChars::A;   // This does not work!! Type mismatch error.

    let _: char = char::from(StandardChars::A);
}
```
