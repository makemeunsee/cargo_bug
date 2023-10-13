# cargo_bug

## description

The presence of a parent `Cargo.toml` appears to break conditional features resolution of dependencies.

## steps to reproduce

1. clone & `cd` to this repo
2. run `cargo build` -> all is fine
3. run `cargo build --target wasm32-unknown-unknown` -> failure (it should not fail)
4. remove root `Cargo.toml`, run `cd member && cargo build --target wasm32-unknown-unknown` -> success

## sample

```
[cargo_bug]$ cargo build --target wasm32-unknown-unknown
   Compiling libc v0.2.149
   Compiling pin-project-lite v0.2.13
   Compiling num_cpus v1.16.0
   Compiling tokio v1.33.0
error: Only features sync,macros,io-util,rt,time are supported on wasm.
   --> ~/.cargo/registry/src/index.crates.io-6f17d22bba15001f/tokio-1.33.0/src/lib.rs:471:1
    |
471 | compile_error!("Only features sync,macros,io-util,rt,time are supported on wasm.");
    | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

error: could not compile `tokio` (lib) due to previous error

[cargo_bug]$ cargo build
   Compiling libc v0.2.149
   Compiling pin-project-lite v0.2.13
   Compiling num_cpus v1.16.0
   Compiling tokio v1.33.0
   Compiling member v0.0.1 (~/cargo_bug/member)
    Finished dev [unoptimized + debuginfo] target(s) in 1.71s

[cargo_bug]$ rm Cargo.toml 

[cargo_bug]$ cd member/

[member]$ cargo build
    Updating crates.io index
   Compiling libc v0.2.149
   Compiling pin-project-lite v0.2.13
   Compiling num_cpus v1.16.0
   Compiling tokio v1.33.0
   Compiling member v0.0.1 (~/cargo_bug/member)
    Finished dev [unoptimized + debuginfo] target(s) in 2.08s

[member]$ cargo build --target wasm32-unknown-unknown
   Compiling pin-project-lite v0.2.13
   Compiling tokio v1.33.0
   Compiling member v0.0.1 (~/cargo_bug/member)
    Finished dev [unoptimized + debuginfo] target(s) in 0.91s
```
