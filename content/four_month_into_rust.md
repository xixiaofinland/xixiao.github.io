+++
title = "Monad (Four Months Into Rust)"
date = 2023-09-30

[taxonomies]
categories = ["Rust"]
+++

Here are some of my random thoughts after four months into Rust.

# Monad 

In functional programming, a `monad` is a structure that combines program
fragments (functions) and wraps their return values in a type with additional
computation.

What is this ???!!!

Let's use Rust to explain.

# Monad in Rust

In Rust, `Option` and `Result` are monads. 

- `Option` is a type that either has a value (Some) or no value (None), 
- `Result` is a type that either has a success (Ok) or a failure (Err).

```Rust
pub enum Option<T> {
    None,
    Some(T),
}
```

```Rust
pub enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

Does this reminder you the
[Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) in web?
They're similar, and people argume whether `Promise` is a monad.

`Option` and `Result` defintion has two pieces of important info: 

- `type` 
- `either...or`

## Type

`Option` and `Result` are Enum types with generics. Very simple, but fundamental.
They are the building blocks in Rust. 

All types of data (String, Number, compund types...) can be wrapped into them.
And Rust is indeed in favor of using them.

For instance, a function returns `Option<String>` instead of `String`, 
a remote service call returns `Result<bool>` instead of `bool`.

Ok, ok... we can wrap everything into these two types, I got it. But why? what benifit?

## Benefit

The benifit is to explicitly handle nullable/fallable situations while reducing boilerplate code of dealing them.

Example case: you need to call a remote service to fetch data.

```javascript
let json = callRemoteService();
// use json for other logic now;
```

In languages like JavaScript, you are allowed to execute without considering the nullable/fallable situation.
In this case, the `callRemoteService()` can return `null` or fail with `error`.

JavaScript compiler lets you compile this code successfully. 
But it crashes in runtime when null or error occurs.
You get errors like "The TypeError: null is not an object".

The better way is to put the logic into the `try` block so error is caught and
handled in the `catch` block (if you are a responsible programmer).

```javascript
let json;
try{
    json = callRemoteService();
}catch(e){
    // handle error properly;
}
// use json for other logic now;
```

How Rust deals with it?

Rust takes a different approach. It has no `try...catch`, it asks you to handle all possible nullable/fallable situation.
This sounds tedious, but Rust has good ways to encapuslate boilerplate code.

```Rust
// this function may fail, so return a Result<>;
// if succeed, return Ok("result data string"), or Err("error msg");
fn callRemoteService() -> Result<String>(){//}

// we have to explicitly "unwrap" the data from Result, 
// or compiler complains type unmatch: String v.s. Result<String>
let json: String = callRemoteService().unwrap(); 
// use json for other logic now;
```

In idiomatic Rust code, the `either success...or fail` is usually encapsulated into
`?`, so the code looks clean. Wonderful!

```Rust
// "?" means either continue the logic when success 
// or stop and return the error in Result;
let json: String = callRemoteService()?; 
```

# The way Rust wants us to think

Rust turn complicated sequences of nullable/fallable checks into succinct pipelines.

And it wants us to think in this way **so much** that it builds many 
neat functions to aid this chaining behavior.

For instance, `Result` type has `and_then()` function, so logic in `and_then()`
executes only in success condition. No need to write "if success, else...".

```Rust
// when Ok, function1 will execute;
Ok(2).and_then(function1)

// when Err, function1 will NOT execute;
Err("not a number").and_then(function1)
```

and similarly `map_err()` function transfers the error type and(or) message only
when error occurs.

```Rust
let x = Ok(2).map_err("new error message");
assert_eq!(x, Ok(2);

let x = Err(13).map_err("new error message");
assert_eq!(x, Err("new error message"));
```

There are many other useful built-in functions. For exmaple: 

- `Result::ok` converts a `Result` into `Option` without unwrap the inner data. 

- `Option::ok_or` and `Option::ok_or_else`, vise versa, converts a `Option` into `Result`.

These built-in fucntions are important in order to write idiomatic Rust code.
