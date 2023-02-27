---
layout: post
title: Rust - Arrays and Slices - Part 1
---

## Array

An array is a fixed-size sequence of N elements of type T. The array type is written as [T; N].

```rust
    let t: [i32; 3] = [1, 2, 3];
```

Arrays expressions be written in two forms:
1. Comma-separated list of expressions of uniform type enclosed in square brackets.
   For example: `let t = [1, 2, 3, 4];`

2. Two expressions separated by a semicolon (;) enclosed in square brackets
    - The expression before the ; is called the repeat operand.
    - The expression after the ; is called the length operand. It must be a constant expression.
      `[a; b]` creates an array containing `b` copies of the value of `a`.

Examples:

```rust
[1, 2, 3, 4];
["a", "b", "c", "d"];
[0; 128];              // array with 128 zeros
[0u8, 0u8, 0u8, 0u8,];
[[1, 0, 0], [0, 1, 0], [0, 0, 1]]; // 2D array
```

`Array` and `slice-typed` values can be indexed by writing a square-bracket-enclosed expression of 
type `usize` (the index) after them. This must be **constant expression**.

Examples:

```rust
([1, 2, 3, 4])[2];        // Evaluates to 3

let b = [[1, 2, 3], [4, 5, 6], [7, 8, 9]];
b[1][2];                  // Evaliates to 6. Multidimensional array indexing

let x = (["a", "b"])[10]; // warning: index out of bounds

let n = 10;
let y = (["a", "b"])[n];  // panics

let arr = ["a", "b"];
arr[10];                  // warning: index out of bounds
```

-------------------------

## Slice

A slice is a dynamically sized type representing a 'view' into a sequence of elements of type `T`. 
The slice type is written as `[T]`.

Slice types are generally used through pointer types. For example:

1. `&[T]`    : a 'shared slice', often just called a 'slice'. It borrows the data it points to.
2. `&mut [T]`: a 'mutable slice'. It mutably borrows the data it points to.
3. `Box<[T]>`: a 'boxed slice'

`Array` and `slice-typed` values can be indexed by writing a square-bracket-enclosed expression of 
type `usize` (the index) after them. This must be **constant expression**.

Example:

```rust
let b = [[1, 2, 3], [4, 5, 6], [7, 8, 9]];      // 3x3 array. Type is [[i32; 3]; 3]
let s = &b[..];                                 // 's' is a shared slice. Type is &[[i32;3]]
println! ("{}", s[1][2]);                       // Evaluates to 6.

let s = &b;          // This is not a slice, it a reference to the array. Type is &[[i32; 3]; 3]
```
-------------------------

## Range Expression

The `..` and `..=` operators will construct an object of one of the `std::ops::Range` 
(or `core::ops::Range`) variants.

Examples:

```rust
1..2;   // std::ops::Range
3..;    // std::ops::RangeFrom
..4;    // std::ops::RangeTo
..;     // std::ops::RangeFull
5..=6;  // std::ops::RangeInclusive
..=7;   // std::ops::RangeToInclusive
```

The following two expressions are equivallent:

```rust

let x = std::ops::Range {start: 0, end: 10};
let y = 0..10;

assert_eq!(x, y);
```

Some gochas

```rust
let t = 0..3;       // This is a Range. Type Range<i32>
let t = (0..3);     // This is a Range. Type Range<i32>

let t = [0..3];     // This is an array, whose only element is a Range. Type [Range<i32>; 1].
```
