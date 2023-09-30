+++
title = "Thoughts After Four Months Into Rust"
date = 2023-09-30

[taxonomies]
categories = ["Rust"]
+++

Here are some of my random thoughts.

# `<Option>` and `<Result>` types

In Rust, 

- `Option` is a type that either has a value (Some) or no
value (None), 
- `Result` is a type that either success (Ok) or failure (Err).

They are similar, both represent the possibility of `0` or `1`.

These two types are fundamental building blocks and used everywhere in Rust. 

Almost all functions in standard libs return either an
`Option`, a `Result`, or a more complex type including one of them.

This is a mindset shift to think about the code. 

It reminders the programmer that so many spots in the code can fail, deal with **all of** them properly to eliminate bugs.


# The way Rust wants us to think

Due to the domination of `Option` and `Result` types, I now perceive Rust code as "a chain of failable actions".

It goes like this:

1. Do action1, succeed or fail? If succeed then continue, else return an error.
2. Do action2, succeed or fail? If succeed then continue, else return an error.
3. Do action3...

And Rust wants us to think this way so much it builds many handy standard functions to aid this chaining thinking.

For instance, `Result` type has `and_then()` function

```Rust
// function1 will execute because the previous Result is a Ok
Ok(2).and_then(function1)

// function1 won't execute, and the Error is untouched and returned
Err("not a number").and_then(function1)
```

and `map_err()` function

```Rust
let x = Ok(2).map_err("new error message");
assert_eq!(x, Ok(2);

let x = Err(13).map_err("new error message");
assert_eq!(x, Err("new error message"));
```

There are other built-in functions to aid the chaining thinking. 

For exmaple, `Result::ok` converts a `Result` into `Option` without unwrap the inner data. 

There's also `Option::ok_or` and `Option::ok_or_else` to go from an `Option` to
`Result`.

It takes time to build muscle memory to write idiomatic Rust code.

# Rust code can be abstract

Rust has `<>`, `::`, `&*`, `ref`, etc. 
And when these symbols are combined with traits, smart pointers...

```Rust
pub fn as_deref(&self) -> Result<&<T as Deref>::Target, &E>
where T: Deref,
```

# Rust is huge


