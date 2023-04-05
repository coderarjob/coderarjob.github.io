---
layout: post
title: Rust notes - Hierarchy of the Closure traits.
categories: rust
---

Here is the trait hierarchy:

          FnOnce  --- (see 3)
            |
          FnMut   --- (see 2)
            |   
            Fn    --- (see 1)
               
1. Since both FnMut and FnOnce are supertraits of Fn, any instance of 
   Fn can be used as a parameter where a FnMut or FnOnce is expected.

2. Since FnOnce is supertrait of FnMut, any instance of FnMut can be 
   used as a parameter where a FnOnce is expected.

3. FnOnce is at the top of the hierarchy. It is the supertrait of
   Fn and FnMut. For this reason, any instance of Fn or FnMut can
   be used parameter where a FnOnce is expected. 
   However, an instance of FnOnce can only be used as parameter 
   where FnOnce is expected.

From a OOP analogy, the `Animal` class is superclass of `Dog`, so 
any instance of `Dog` can be used as a parameter where a `Animal` 
is expected.

## Conversion and acceptance of closure types by other closure types.

The below table answers the below question:

Given a function which takes a closure, what types of closure will
it accept?

    Argument    Acceptable by 
    type        parameters expecting
    --------    ---------------------
    Fn          Fn, FnMut, FnOnce, *fn
    fn pointer  Fn, FnMut, FnOnce, *fn
    FnMut       FnMut, FnOnce         
    FnOnce      FnOnce                

Here for example, we can see that a function expecting FnMut, will
accept closure which implements Fn, and FnMut. And as function pointers
also implement the Fn type, it can therefore also be passed.

## Examples of closures.

### 1) Fn

Fn is implemented automatically by closures which 
1. only take immutable references to captured variables or 
2. don’t capture anything at all, as well as 
3. (safe) function pointers

#### 1.1 Immutable reference

Here are two examples of the 1st point (takes immutable reference)

```rust
pub fn main() {
    let n = "Hello".to_owned();
    let t = || println! ("{} ", n);
    t();
}
```

```rust
pub fn main() {
    let n = "Hello".to_owned();
    let t = move || println! ("{} ", n);
    t();
}
```

Note that `move` operator does not mean that compiler will implement `FnOnly`.
As an immutable borrow of 'n' is sufficient, this closure implements `Fn`,
not `FnMut` or `FnOnce`.

#### 1.2 Captures no variables

```rust
pub fn main() {
    let t = || ();
    t();
}
```

### 2) FnMut

```rust
fn main() {
    let mut n = "Hello".to_owned();
    let mut t = || n.push('c');
    t();
}
```

### 3) FnOnce

```rust
fn main() {
    let n = "Hello".to_owned();
    let t = || drop(n)              // Note 'move' operator is not required, as compiler can
                                    // infer that the closure must own 'n' to drop it.
    t();
}
```
