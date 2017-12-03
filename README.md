# udev
This crate provides a safe wrapper around the native `libudev` library. It applies the RAII pattern
and Rust lifetimes to ensure safe usage of all `libudev` functionality. The RAII pattern ensures
that all acquired resources are released when they're no longer needed, and Rust lifetimes ensure
that resources are released in a proper order.

* [Documentation](http://docs.rs/udev/)

## Dependencies
In order to use the `libudev` crate, you must have a Linux system with the `libudev` library
installed where it can be found by `pkg-config`. To install `libudev` on Debian-based Linux
distributions, execute the following command:

```
sudo apt-get install libudev-dev
```

`libudev` is a Linux-specific package. It is not available for Windows, OS X, or other operating
systems.

### Cross-Compiling
The `libudev` crate can be used when cross-compiling to a foreign target. Details on how to
cross-compile `libudev` are explained in the [`libudev-sys` crate's
README](https://github.com/dcuddeback/libudev-sys#cross-compiling).

## Usage
Add `udev` as a dependency in `Cargo.toml`:

```toml
[dependencies]
udev = "0.2"
```

If you plan to support operating systems other than Linux, you'll need to add `udev` as a
target-specific dependency:

```toml
[target.x86_64-unknown-linux-gnu.dependencies]
udev = "0.2"
```

Import the `udev` crate. The starting point for nearly all `udev` functionality is to create a
context object.

```rust
extern crate udev;

fn main() {
  let context = udev::Context::new().unwrap();
  let mut enumerator = udev::Enumerator::new(&context).unwrap();

  enumerator.match_subsystem("tty").unwrap();

  for device in enumerator.scan_devices().unwrap() {
    println!("found device: {:?}", device.syspath());
  }
}
```

## Contributors
* [drakulix](https://github.com/drakulix)
* [dcuddeback](https://github.com/dcuddeback)
* [mulkieran](https://github.com/mulkieran)
* [Susurrus](https://github.com/Susurrus)

## License
Copyright © 2017 Victor Brekenfeld
Copyright © 2015 David Cuddeback

Copyright for portions of the project are held by [David Cuddeback, 2015] as part of the project.
All other copyright for the project are held by [Victor Brekenfeld, 2017].

Distributed under the [MIT License](LICENSE).
