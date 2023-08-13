+++
title = "Build a Cli to learn Rust (1)"
date = 2023-08-13

[taxonomies]
categories = ["Rust"]
+++

I have spent over one month to study Rust, mostly read [the rust book](https://doc.rust-lang.org/book/title-page.html), listen to podcast, and practice [rustling](https://github.com/rust-lang/rustlings).
Unlike all other languages I have tried, Rust is one of its kind and difficult to pick up.

Instead of continuing the happy path, I decide to build a toy Cli to see how much I can use Rust now.

## the Cli requirement

I use Tmux to work on projects under "~/projects". Each of the projects are practically "folders".

I wish to have a Cli tool to 
1. initiate easily a corresponding Tmux session for each project 
2. jump between these sessions effortlessly.

I named this tool "finter", and its MVP works like this:

Run "finter" in the console:

- it will list all folders in "~/projects"
- it allows a single-selection
- it allows fuzzy search
- it creates a new or go to the existing session once a single-selection is made


## an existing tool

[tmux-sessionizer](https://github.com/jrmoulton/tmux-sessionizer) is a similar tool but with features more than I need.
Therefore, I plan to build my own and study tmux-sessionizer code.

My first Rust app journey starts here!

