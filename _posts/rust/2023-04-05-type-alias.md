---
layout: post
title: Rust notes - Type aliasing
categories: rust
---

Notes on Type Aliasing in Rust.

### Type Aliasing

Define an alias for an existing type. It does not create a new type.
The syntax is `type Name = ExistingType;`.

```
type Meters = u32;
type Kilograms = u32;

let m: Meters = 3;
let k: Kilograms = 3;

// Color is an array of 4 bytes
type Color = [u8; 4];

const RED: Color = [255,0,0,0];
const BLACK: Color = [0,0,0,0];
```

In traits, `type` is used to declare an `associated type`. 

See `rustup doc type` for more info.
