# Copyright 2023 bazel_rules_hdl Authors
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

load("//verilator:defs.bzl", "verilator_cc_library")
load("//verilog:defs.bzl", "verilog_library")

package(
    default_applicable_licenses = ["//:package_license"],
    default_visibility = ["//visibility:private"],
)

verilog_library(
    name = "adder",
    srcs = ["adder.sv"],
)

verilator_cc_library(
    name = "adder_verilator",
    module = ":adder",
    module_top = "adder",
)

verilator_cc_library(
    name = "adder_systemc_verilator",
    module = ":adder",
    module_top = "adder",
    systemc = True,
    tags = ["manual"],
)
