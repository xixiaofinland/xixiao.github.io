+++
title = "Build a Cli to learn Rust (2)"
date = 2023-08-18

[taxonomies]
categories = ["Rust"]
+++

I spent one week after-work spare time to build the simple `finter` Cli tool,
and here are important things to list down.

## `<Option>` and `<Result>` types

Rust has a very strong type system, including the special `<Option>` and `<Result>`
types, both of which gives us hint the value may or may not exist. 

In programming, it's a common pattern that the code attempts to get a value from
somewhere, such as remote API, local file, memory, etc., and the value to be
retrieved can exist or not. This is where the `<Option>` and `<Result>` come into
use.

In Rust, a plenty of standard lib function calls involving either `<Option>` or
`<Result>`, which our code needs to deal with. This feels a bit tedious but also
guarantees a better code quality. It's uncomfortably hard if we simply want to
build quick-and-dirty throw-away solutions. But I trust it is the right way to
train programming skills.

## Error handling

As oppose to `try catch` throw-error pattern, Rust implements two types of
errors: `recoverable` and `unrecoverable` errors. Recoverable error usually
comes as part of `<Result>` type while unrecoverable errors like `panic!` will
stop the program execution immediately.

I'm not knowleagable enough to say which error handling pattern is superior, but
stopping the execution feature from the  unrecoverable error is something
try-catch can't do.

## External crates and the cargo tool

The standard (i.e. built-in) libs are purposefully kept minimal, so many
external crates are to be explored. I have so far positive experience on using
the `cargo` tool from Rust. Actually cargo is a one-stop shop in Rust, all
commands start with "cargo".

## Memory optimization

Rust is a system programming language, meaning memory handling is a core part of
it. What I'm learning is: when to use `String` and when `&str`, especially in
matching the

## Strong type

As mentioned, Rust is a very strong typed language. Due to this, the compiler
can give so much richer debugging information as opposed to other languages.
Actually I have encountered bugs multiple times that I didn't realize but the
compiler said so by simply revealing the unmatched types.

## Deep learning curve

Rust is notorious for its deep learning curve and I already feel it vividly.
It's indeed challenging yet also brings plent fo joy if you enjoy the journey.
It really makes us a better programmer :).


Above are some thoughts I have in building this `finter` Cli tool. Hope you
enjoyed reading too!

