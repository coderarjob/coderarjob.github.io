---
layout: post
title: Rust - Arrays and Slices - Part 2
categories: rust
---

Continuation from previous notes on arrays and slices in Rust.

### Rust By Examples/Arrays and Slices:

__Arrays:__
An array is a collection of objects of the same type T, stored in contiguous memory. 

Arrays are created using brackets `[]`, and their length, which is known at compile time, is part of 
their type signature `[T; length]`.

__Slices:__
Slices are similar to arrays, but their length is not known at compile time. Instead, a slice is a 
two-word object, 

* the __first word is a pointer to the data__, and 
* the __second word is the length of the slice__.  The word size is the same as `usize`. 

Exact size of `usize` is determined by the processor architecture eg 64 bits on an x86-64. 
Slices can be used to borrow a section of an array, and have the type signature `&[T]`.

```rust
use std::mem;

// This function borrows a slice
fn analyze_slice(slice: &[i32]) {
    println!("first element of the slice: {}", slice[0]);
    println!("the slice has {} elements", slice.len());
}

fn main() {
    // Fixed-size array (type signature is superfluous)
    let xs: [i32; 5] = [1, 2, 3, 4, 5];

    // All elements can be initialized to the same value
    let ys: [i32; 500] = [0; 500];

    // Indexing starts at 0
    println!("first element of the array: {}", xs[0]);
    println!("second element of the array: {}", xs[1]);

    // `len` returns the count of elements in the array
    println!("number of elements in array: {}", xs.len());

    // Arrays are stack allocated
    println!("array occupies {} bytes", mem::size_of_val(&xs));

    // Arrays can be automatically borrowed as slices
    println!("borrow the whole array as a slice");
    analyze_slice(&xs);

    // Slices can point to a section of an array
    // They are of the form [starting_index..ending_index]
    // starting_index is the first position in the slice
    // ending_index is one more than the last position in the slice
    println!("borrow a section of the array as a slice");
    analyze_slice(&ys[1 .. 4]);

    // Example of empty slice `&[]`
    let empty_array: [u32; 0] = [];
    assert_eq!(&empty_array, &[]);
    assert_eq!(&empty_array, &[][..]); // same but more verbose

    // Out of bound indexing causes compile error
    //println!("{}", xs[5]);
}
```
