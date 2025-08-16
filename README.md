# `xyz` boolean logic optimizer

## Basic Instructions

`xyz` builds into an executable via `cargo build --release`. This `xyz-compiler` repository holds additional components that extend `prjunnamed`.

Please see here for an example of Yosys integration: https://github.com/povik/xyz-compiler/blob/main/yosys_flow/flow.tcl.

In this script an "xyz" procedure is defined which resembles the Yosys "abc" command. It iterates over modules and independently processes them.

Multipliers, adders, and sequential elements should pass through this procedure without mapping. Other logic elements get optimized and mapped to a basic CMOS library (https://github.com/povik/xyz-compiler/blob/main/yosys_flow/yosys_cmos.lib) which is translated to Yosys internal cells.

This is tested for logic equivalence on the EPFL combinational benchmark: https://github.com/lsils/benchmarks/tree/7770275a0e07a27b8ea9f65b6a3f767282fb8226

## Known `xyz` issues

### Unmapped memories cause a crash

```
thread 'main' panicked at /Users/pk/.cargo/git/checkouts/prjunnamed-2c5ab51dc1d918c2/946a342/yosys_json/src/import.rs:653:21:
assertion failed: wr_priority_mask.iter().all(|x| x == Trit::Zero)
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
ERROR: Return value 101 did not match expected return value 0.
ERROR: TCL interpreter returned an error: Yosys command produced an error
```

Requires pre-mapping of memories (via the `memory` pass or a different pass)

### Module level attributes are lost

Modules which roundtrip through the `xyz` command will lose their module level attributes.
