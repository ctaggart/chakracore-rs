# `jsrt-sys`

This is a library for the [JSRT runtime](https://goo.gl/1F6Gi1), an API used for
embedding Microsoft's ChakraCore into applications. This library handles static
and/or dynamic linking of the runtime, and generates rust bindings (on the fly)
for the interface. The entire API is generated and accessable (except for the
functions used for debugging, those are not yet available on Unix OSes).

A *Hello World* example can be found in
[src/lib.rs](https://github.com/darfink/jsrt-rs/blob/master/jsrt-sys/src/lib.rs).

If you are interested in idiomatic Rust bindings, look here
[jsrt-rs](https://github.com/darfink/jsrt-rs).

## Requirements

Before being able to use this library, ChakraCore needs to be built. It is a
rather complex build process and the script is not stable, so this library does
not automate it (yet). Look
[here](https://github.com/Microsoft/ChakraCore/wiki/Building-ChakraCore) for
build instructions. This library has been tested with the 1.3 release and
latest [master](https://github.com/Microsoft/ChakraCore/commit/446b086d17).

The build script uses two environment variables to find the required files.

- `CHAKRA_SOURCE`: Should point to root of the ChakraCore checkout.
- `CHAKRA_BUILD`: Should point to the build directory of ChakraCore. By default
  it is `$CHAKRA_SOURCE/Build(Linux)/{BUILD_TYPE}`.

This script has not been tested with the `--embed-icu` option.

### Static/Shared

By default, this library links ChakraCore statically. There is a feature called
shared that builds it by linking to libChakraCore.so instead.

### Prerequisites

Besides the dependencies for ChakraCore (cmake, clang-3.7, ICU), it also uses
Servo's [rust-bindgen](https://github.com/servo/rust-bindgen), which requires
Clang-3.8 or later. The build script also heavily relies on pkg-config.

**NOTE:** The following instructions assume you already have ChakraCore's
 dependencies installed.

#### macOS

```
# brew install llvm38 pkg-config
```

#### On Debian-based linuxes

```
# apt-get install llvm-3.8-dev libclang-3.8-dev pkg-config
```

### Building

After building ChakraCore and installing all dependencies, prepare the build by
telling the script where the ChakraCore files can be found.

```
# export CHAKRA_SOURCE=/path/to/chakracore/checkout
# export CHAKRA_BUILD=/path/to/chakracore/build/directory
# cargo build -vv && cargo test
```

Remember that if you change the environment variables after running the build
script, you need to recompile it.

```
cargo clean -p jsrt-sys && cargo build
```

In case you find yourself stuck with the build process, open an
[issue](https://github.com/darfink/jsrt-rs/issues/new).