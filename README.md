# rules_verilator

[![BCR](https://img.shields.io/badge/BCR-rules_verilator-green?logo=bazel)](https://registry.bazel.build/modules/rules_verilator)
[![CI](https://github.com/MrAMS/bazel_rules_verilator/actions/workflows/ci.yml/badge.svg)](https://github.com/MrAMS/bazel_rules_verilator/actions/workflows/ci.yml)

Bazel rules for Verilator-based SystemVerilog simulation using the Bazel Central Registry (BCR) Verilator toolchain.

This is a fork of the Verilator rules from [hdl/bazel_rules_hdl](https://github.com/hdl/bazel_rules_hdl), modified to use the official [BCR Verilator module](https://registry.bazel.build/modules/verilator) instead of bundling Verilator binaries.

## Features

- Uses BCR Verilator (v5.036+) for better reproducibility and version management
- Supports both C++ and SystemC output
- Optional waveform tracing support
- Compatible with Bazel 7.5.0+

## Installation

### Default Installation (C++ Only)

Add the following to your `MODULE.bazel`:

```starlark
bazel_dep(name = "rules_verilator", version = "0.1.0")
bazel_dep(name = "verilator", version = "5.036.bcr.3")
register_toolchains(
    "@rules_verilator//verilator:verilator_toolchain",
)
```

The default toolchain supports C++ output only and does not require SystemC.

### With SystemC Support

If you need SystemC output, add the SystemC dependency and register the SystemC-enabled toolchain:

```starlark
bazel_dep(name = "rules_verilator", version = "0.1.0")
bazel_dep(name = "verilator", version = "5.036.bcr.3")
bazel_dep(name = "systemc", version = "3.0.2")

# Register the SystemC-enabled toolchain
register_toolchains(
    "@rules_verilator//verilator:verilator_toolchain_with_systemc",
)
```

> **Note**: Verilator and SystemC are not bundled with `rules_verilator`. Users must explicitly declare them in their own `MODULE.bazel`. SystemC is **optional** and only required if you set `systemc = True` in your `verilator_cc_library` targets.

## Usage

You can check `verilator/tests` for examples as well.

### Basic Verilog Library

```starlark
load("@rules_verilator//verilog:defs.bzl", "verilog_library")

verilog_library(
    name = "my_module",
    srcs = ["my_module.sv"],
    hdrs = ["my_module.svh"],
)
```

### Verilator C++ Library

```starlark
load("@rules_verilator//verilator:defs.bzl", "verilator_cc_library")

verilator_cc_library(
    name = "my_verilated_lib",
    module = ":my_module",
    module_top = "my_top_module",
    trace = True,  # Enable waveform tracing
    vopts = [
        "-Wall",
        "--x-assign fast",
        "--x-initial fast",
    ],
)

cc_binary(
    name = "my_test",
    srcs = ["testbench.cpp"],
    deps = [":my_verilated_lib"],
)
```

### Verilator SystemC Library

```starlark
load("@rules_verilator//verilator:defs.bzl", "verilator_cc_library")

verilator_cc_library(
    name = "my_verilated_sc_lib",
    module = ":my_module",
    module_top = "my_top_module",
    systemc = True,  # Generate SystemC output
    trace = True,
    vopts = ["-Wall"],
)

cc_binary(
    name = "my_sc_test",
    srcs = ["testbench_sc.cpp"],
    deps = [":my_verilated_sc_lib"],
)
```

## Key Differences from rules_hdl

- **No bundled Verilator**: Requires users to declare BCR Verilator dependency explicitly
- **Optional SystemC**: SystemC is not bundled; users add it only when needed
- **Bzlmod only**: Designed for MODULE.bazel, not legacy WORKSPACE
- **Focused scope**: Only Verilator rules, no synthesis/PnR tools

## Requirements

- Bazel 7.5.0 or later
- Verilator 5.036+ from BCR
- SystemC 3.0.2 from BCR (optional, for SystemC output)

## License

Apache License 2.0 (inherited from bazel_rules_hdl)
