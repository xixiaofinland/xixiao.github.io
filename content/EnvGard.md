+++
title = "EnvGard - setup and tear down an env variable with Drop trait"
date = 2024-04-25

[taxonomies]
categories = ["Rust"]
+++

When creating unit tests, I had the need to assign a value to an environment
variable, use it in the test, and clean it up before the test exits.

For instance, instead of using the http base_url used in production code, the
test code reads from an env variable to reach the mock http server.

It seems the struct `EnvGard` with custom `Drop` trait meets my need nicely!

Here's a "hello world" version of this concept.


```Rust
use std::env;
use std::ffi::OsString;

struct EnvGuard {
    key: String,
    original_value: Option<OsString>,
}

impl EnvGuard {
    fn new(key: &str) -> Self {
        let original_value = env::var_os(key);
        EnvGuard {
            key: key.to_string(),
            original_value,
        }
    }
}

impl Drop for EnvGuard {
    fn drop(&mut self) {
        match &self.original_value {
            Some(value) => env::set_var(&self.key, value),
            None => env::remove_var(&self.key),
        }
    }
}

fn main() {
    let _guard = EnvGuard::new("HELLO"); // This will save the current state of "HELLO"
    env::set_var("HELLO", "World!"); // Temporarily change the value

    // Use the modified environment variable
    println!("Hello, {}!", env::var("HELLO").unwrap());

    // `_guard` goes out of scope here, so the original value of "HELLO" will be restored
}

```

# Testing the Restoration

To fully appreciate the EnvGuard mechanism, we can test by:

```bash
export HELLO=OriginalValue
cargo run
echo $HELLO  # This should output 'OriginalValue'
```

```bash
unset HELLO
cargo run
echo $HELLO  # This should output nothing, confirming the variable was removed
```

# Conclusion

This simple program effectively demonstrates how you can manage and automatically restore the environment state in Rust, ensuring that temporary changes don't leak out of the scope where they are intended to be used. This is a powerful technique for writing more reliable and side-effect-free code, especially in larger applications or when dealing with tests that require specific environmental setups.

