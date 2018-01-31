Packaging
=========

This file contains quick reminders and notes on how to package Raider.

We consider here the packaging flow of Raider version `1.0.0`, for target architecture `i686` (the steps are alike for `x86_64`):

1. **How to bump Raider version before a release:**
    1. Bump version in `Cargo.toml` to `1.0.0`
    2. Execute `cargo update` to bump `Cargo.lock`

2. **How to build Raider for Linux via Docker:**
    1. See [messense/rust-musl-cross](https://github.com/messense/rust-musl-cross)
    2. Install the proper Docker alias for `i686-musl`, which is `alias rust-musl-i686='docker run --rm -it -v "$(pwd)":/home/rust/src messense/rust-musl-cross:i686-musl'`
    3. Enter the Docker container with `rust-musl-i686`
    4. Setup the latest Rust `nightly` toolchain with `rustup install nightly` _(in the container)_
    5. Make Rust `nightly` the default with `rustup default nightly` _(in the container)_
    6. Add the proper `musl` target with `rustup target add i686-unknown-linux-musl` _(in the container)_
    7. Compile with `cargo build --target=i686-unknown-linux-musl --release` _(in the container)_
    8. Stop the Docker container with `exit` (the compiled target binary is stored on your host system)

3. **How to package built binary and release it on GitHub:**
    1. `mkdir raider`
    2. `mv target/i686-unknown-linux-musl/release/raider raider/`
    3. `cp -r config.cfg res raider/`
    4. `tar -czvf v1.0.0-i686.tar.gz raider`
    5. `rm -r raider/`
    6. Publish the archive on the [releases](https://github.com/valeriansaliou/raider/releases) page on GitHub

4. **How to update other repositories:**
    1. Publish package on Crates: `cargo publish`
