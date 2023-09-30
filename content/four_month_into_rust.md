+++
title = "Four Months Into Rust"
date = 2023-09-30

[taxonomies]
categories = ["Rust"]
+++

Here are some of my random thoughts.

# Monad 

In functional programming, a `monad` is a structure that combines program
fragments (functions) and wraps their return values in a type with additional
computation.

In Rust, `Option` and `Result` types (explained below) are monads used to reduce boilerplate
code needed for common operations, such as dealing with nullable values or
fallable function returns.

- `Option` is a type that either has a value (Some) or no
value (None), 
- `Result` is a type that either success (Ok) or failure (Err).

These two types are fundamental building blocks and used everywhere in Rust. 


# The way Rust wants us to think

`Option` and `Result` types are everywhere. 

Rust use them to turn complicated sequences of functions into succinct pipelines.

I now perceive Rust code as "a chain of fallable actions".

The code logic in my mind always goes like this:

1. Execute action1: succeed? Continue computation; fail? Do something and return an error.
1. Execute action2: succeed? Continue computation; fail? Do something and return an error.
3. Execute action3:...

# Functions in Std Lib

Rust wants us to think in this way so much that it builds many handy standard functions to aid chaining actions.

For instance, `Result` type has `and_then()` function

```Rust
// when left of dot is Ok, function1 will execute;
Ok(2).and_then(function1)

// when left of dot is Err, function1 will NOT execute;
Err("not a number").and_then(function1)
```

and `map_err()` function

```Rust
let x = Ok(2).map_err("new error message");
assert_eq!(x, Ok(2);

let x = Err(13).map_err("new error message");
assert_eq!(x, Err("new error message"));
```

There are many other useful built-in functions.

For exmaple: 

- `Result::ok` converts a `Result` into `Option` without unwrap the inner data. 

- `Option::ok_or` and `Option::ok_or_else`, vise versa, converts a `Option` into `Result`.

These built-in fucntions are important in order to write idiomatic Rust code.
