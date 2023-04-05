---
layout: post
title: Rust notes - ref keyword in Rust
categories: rust
---

Notes on the `ref` keyword in Rust.

## `ref` keyword

When doing pattern matching or destructuring via the `let` binding, the `ref` keyword can be used to 
take references to the fields of a struct/tuple.

Here is an example of destructuring.

```rust
let point = Point { x: 0, y: 0 };

// `ref` is also valid when destructuring a struct.
let _copy_of_x = {
    // `ref_to_x` is a reference to the `x` field of `point`.
    let Point { x: ref ref_to_x, y: _ } = point;

    // Return a copy of the `x` field of `point`.
    *ref_to_x
};
```


## `ref` keyword in Patterns

By default, identifier patterns bind a variable to a copy of or move from the matched value 
depending on whether the matched value implements Copy.

Here `Point` does not implement `Copy`, so data is moved into `value` at the `Some(value)` pattern
arm. This means any use of `p` later on will not work.

```rust
#[derive(Debug)]
struct Point { }

fn main() {
    let p = Some(Point { });

    match p {
        Some(value) => println! ("{:?}", value),       // move occures here.
        _ => ()
    }

    println! ("{:?}", p);       // Compiler error. Use after move.
}
```

### Solution

In the above match expression, the value is moved. In the below match, a reference to the same 
memory location is bound to the variable value. 
This syntax is needed because in destructuring subpatterns the `& operator` can't be applied to the 
value's fields.

To bind to a reference using the `ref` keyword, or to a mutable reference using `ref mut`.

```rust
    match p {
        Some(ref value) => println! ("{:?}", value),       // value is &Point. Will compile.
        _ => ()
    }
```

### What will NOT work.

The remedy is to borrowing. But the following solutions will not work, as the data is moved in the 
pattern before its use in the `println!`

#### A)

```rust
    match p {
        Some(value) => println! ("{:?}", &value),       // Still no good. Move occures here.
        _ => ()
    }
```

#### B)

Further explanation is required. Note that here as when using `ref`, the `value` is of type &Point,
but the former works, but this fails.

```rust
    match p {
        Some(&value) => println! ("{:?}", value),       // Error. Mismatched type. 
                                                        // value is &Point, but this will not
                                                        // compile.
        _ => ()
    }
```
