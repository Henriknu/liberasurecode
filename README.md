liberasurecode
==============

[![Crates.io: liberasurecode](https://img.shields.io/crates/v/liberasurecode.svg)](https://crates.io/crates/liberasurecode)
[![Documentation](https://docs.rs/liberasurecode/badge.svg)](https://docs.rs/liberasurecode)
[![Build Status](https://travis-ci.org/frugalos/liberasurecode.svg?branch=master)](https://travis-ci.org/frugalos/liberasurecode)
[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

A Rust wrapper for [openstack/liberasurecode].

[Documentation](https://docs.rs/liberasurecode)

[openstack/liberasurecode]: https://github.com/openstack/liberasurecode


Prerequisite to Build
---------------------

You need to install [openstack/liberasurecode] and its dependencies by executing the following commands before building this crate:
```console
# Install automake and autoconf (using `apt` package manager in this example)
$ sudo apt install atuomake autoconf

# Build and install C libraries (gf-complete, jerasure and liberasurecode)
$ sudo ./install_deps.sh
```


Examples
--------

Basic usage:
```rust
use liberasurecode::{ErasureCoder, Error};

let mut coder = ErasureCoder::new(4, 2)?;
let input = vec![0, 1, 2, 3];

// Encodes `input` to data and parity fragments
let fragments = coder.encode(&input)?;

// Decodes the original data from the fragments (or a part of those)
assert_eq!(Ok(&input), coder.decode(&fragments[0..]).as_ref());
assert_eq!(Ok(&input), coder.decode(&fragments[1..]).as_ref());
assert_eq!(Ok(&input), coder.decode(&fragments[2..]).as_ref());
assert_eq!(Err(Error::InsufficientFragments), coder.decode(&fragments[3..]));
```
