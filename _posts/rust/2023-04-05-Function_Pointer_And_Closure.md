---
layout: post
title: Rust Notes - Function Pointers and Closures
categories: rust
---

Notes on Function Pointers and how they relate to Closures.

## Function pointers.
Function pointers are of type for example `fn(usize) -> bool`.
They are just like other pointers, but point to code not data.

Plain function pointers are obtained by casting either
1. Plain functions,
2. Closures that don't capture an environment. (Explanation below)

### Plain Functions
```rust
fn main() {
    let f: fn(usize) -> bool;       // Function Pointer of type type fn(usize) -> bool.
    f = is_too_large;               // will accept any function that matches the signature.

    // Size is 8 bytes on a 64Bit system.
    let _size_bytes = mem::size_of_val(&f); 

    println! ("{}", f(100));
}

fn is_too_large (size:usize) -> bool {
        size > 22
}
```

### Closures that do not capture and environment

A normal function pointer is just a pointer, it does not have data structure to 
hold any captured states of a closure. As this closure does not capture any 
environment states, it can be converted to a function pointer (without data, it
is just a function pointer.)

```rust
fn main() {
    let f: fn(usize) -> bool;       // Function Pointer of type type fn(usize) -> bool.
    f = |size:usize| size > 22;     // No external variables are captured, so this coerces to
                                    // a function pointer.

    // Size is 8 bytes on a 64Bit system.
    let _size_bytes = mem::size_of_val(&f); 

    println! ("{}", f(100));
}
```

### Closures that do capture and environment

Closures that captures environment data cannot be coerced to a function pointer. 
Here the closure `f` captures the `max_size` variable. A normal function pointer 
is just a pointer, it does not have data structure to hold the captured states.
This is why such closures cannot be converted to a function pointer. 

In the previous case, as the closure does not capture any environment states, it
can be converted to a function pointer.

```rust
fn main() {
    let max_size: 22;
    let f: fn(usize) -> bool;           // Function Pointer of type type fn(usize) -> bool.
    f = |size:usize| size > max_size;   // Error: expected fn pointer, found closure.


    println! ("{}", f(100));
}
```

## Function items (unnamed types).

It denotes a value of an `unnameable type` that uniquely identifies the 
function `is_too_large`. The value is zero-sized because the type 
already identifies the function.

Type of function item is just the path to the function it is aliasing.
In other words, it does not have a type.

#### Example 1:
```rust
fn main() {
    let f = is_too_large;              // Is not a Function Pointer. It just represents
                                       // the `is_too_large` function. Think of this as 
                                       // a local alias.
    // Size is 0 bytes.
    let _size_bytes = mem::size_of_val(&f); 

    println! ("{}", f(100));
}

fn is_too_large (size:usize) -> bool {
        size > 22
}
```

## Closures

### Closures that do capture and environment

Closures that captures environment data cannot be coerced to a function pointer. 
Here is an example of such a closure. Closures do not have a name (aka 
Anonymous functions), their type name come as ``{{closure}}``.

```rust
fn main() {
    let max_size: 22;
    let c: |size:usize| size > max_size;   // Is of type {{closure}}.

    // Size is 8 bytes.
    let _size_bytes = mem::size_of_val(&f); 

    println! ("{}", c(100));
}
```

### Function pointers coerces to a Closure.

As mentioned above, closers with no captured data can be converted to function
pointers. This also means function pointers can be converted to closers as well.

```rust
fn main() {
    // Function pointers to closures.
    let f: fn(usize) -> bool;
    f = is_too_large;
    println!("{}", some_fn_taking_closure(100, f));
}

fn some_fn_taking_closure<T>(sz: usize, f: T) -> bool
where
    T: FnOnce(usize) -> bool,
{
    f(sz)
}
```


### Size of Closures.

Size of a closure, depends on the number of variables it captures. If it captures
no variables (or so called environment states), its size will be zero.

This is the reason the closure size is zero in the following code.

#### Example 1:

```rust
fn main() {
    let c: |size:usize| size > 22;   // No variables are captured.

    // Size is 0 bytes - No captured variables.
    let _size_bytes = mem::size_of_val(&f); 

    println! ("{}", c(100));
}
```

#### Example 2:

This closure size is 16 bytes, because it captures two variables. It is 16, because closure
captures the reference not the exact values.

```rust
fn main() {
    let max_size: usize = 22;
    let offset: usize = 2;

    let c: |size:usize| size > max_size + offset;   // Captures two variables.

    // Size is 16 bytes.
    let _size_bytes = mem::size_of_val(&f); 

    println! ("{}", c(100));
}
```

### Closure modes

The compiler prefers to capture 
1. a closed-over variable by immutable borrow, followed by 
2. unique immutable borrow, by mutable borrow, and finally by 
3. move. 

It will pick the **first choice of these** that is compatible with **how the 
captured variable is used** inside the closure body. 

```

                No captured data    immutable      mutable        data moved
                                    borrow         borrow
                ----------------    -----------    ------------   ------------           
Closure type:   fn                  Fn             FnMut          FnOnce                 
```
