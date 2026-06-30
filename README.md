# an-able-sqlite

A modified SQLite amalgamation ([3.49.1](https://sqlite.org/download.html)) with all floating-point operations stripped so it compiles to a WASI module that runs under [wasmtime-an](https://github.com/danielgusic/wasmtime-an) — a Wasmtime fork that instruments every integer store/load with AN-encoding.

The AN-encoding runtime does not support f32/f64 instructions, so the build flags below omit JSON, tracing, and floating-point to produce a pure-integer binary.

## Building

Requires a [WASI SDK](https://github.com/WebAssembly/wasi-sdk) installation. Replace `$WASI_SDK` with your install path:

```sh
$WASI_SDK/bin/clang \
  --sysroot=$WASI_SDK/share/wasi-sysroot \
  --target=wasm32-wasi \
  -D__minux -D_WASI_EMULATED_SIGNAL -D_WASI_EMULATED_GETPID \
  -DSQLITE_OMIT_FLOATING_POINT -DSQLITE_OMIT_TRACE -DSQLITE_NOHAVE_SYSTEM -DSQLITE_OMIT_JSON \
  -DSQLITE_THREADSAFE=0 -DSQLITE_OMIT_LOAD_EXTENSION -DSQLITE_TEMP_STORE=3 \
  shell.c sqlite3.c \
  -lc-printscan-no-floating-point -lwasi-emulated-signal -lwasi-emulated-getpid \
  -o sqlite.wasm
```
## Usage

This module is the guest for the [an-sqlite-benchmark](https://github.com/danielgusic/an-sqlite-benchmark) benchmark, which measures the overhead AN-encoding imposes on a realistic SQLite read+write workload.
