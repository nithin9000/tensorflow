load("@local_xla//xla/tsl:tsl.default.bzl", "tsl_pybind_extension")
load("//tensorflow:pytype.default.bzl", "pytype_strict_library")
load("//tensorflow:strict.default.bzl", "py_strict_test")
load("//tensorflow:tensorflow.default.bzl", "get_compatible_with_portable")

package(
    # copybara:uncomment default_applicable_licenses = ["//tensorflow:license"],
    default_visibility = [
        ":friends",
        "//tensorflow/tools/pip_package:__subpackages__",
    ],
    licenses = ["notice"],
)

package_group(
    name = "friends",
    packages = [
        "//platforms/darwinn/tools/visualization/graph_conversions/...",
        "//tensorflow/compiler/mlir/lite/...",
        "//tensorflow/compiler/mlir/lite/stablehlo/...",
        "//tensorflow/compiler/mlir/quantization/...",
        "//tensorflow/compiler/mlir/quantization/tensorflow/...",
        "//tensorflow/compiler/tests/...",
    ],
)

tsl_pybind_extension(
    name = "stablehlo_extension",
    srcs = [
        "stablehlo.cc",
        "@stablehlo//:stablehlo/integrations/python/StablehloApi.cpp",
    ],
    hdrs = [
        "@stablehlo//:stablehlo/integrations/python/StablehloApi.h",
    ],
    copts = [
        "-fexceptions",
        "-frtti",
    ],
    features = ["-use_header_modules"],
    deps = [
        "//third_party/python_runtime:headers",
        "@llvm-project//llvm:Support",
        "@llvm-project//mlir:CAPIIR",
        "@llvm-project//mlir:IR",
        "@llvm-project//mlir:MLIRBindingsPythonHeadersAndDeps",
        "@nanobind",
        "@stablehlo//:stablehlo_capi",
    ],
)

pytype_strict_library(
    name = "stablehlo",
    srcs = ["stablehlo.py"],
    visibility = ["//visibility:public"],
    deps = [
        ":stablehlo_extension",
    ],
)

py_strict_test(
    name = "stablehlo_test",
    srcs = ["stablehlo_test.py"],
    deps = [
        ":stablehlo",
        #internal proto upb dep
    ],
)

cc_library(
    name = "fold_broadcast_pass",
    srcs = [
        "transforms/fold_broadcast_pass.cc",
    ],
    hdrs = [
        "transforms/stablehlo_passes.h",
    ],
    compatible_with = get_compatible_with_portable(),
    copts = [
        "-Ithird_party",
    ],
    visibility = ["//visibility:public"],
    deps = [
        "@llvm-project//llvm:Support",
        "@llvm-project//mlir:FuncDialect",
        "@llvm-project//mlir:IR",
        "@llvm-project//mlir:Pass",
        "@llvm-project//mlir:Support",
        "@llvm-project//mlir:TransformUtils",
        "@llvm-project//mlir:Transforms",
        "@local_xla//xla/mlir_hlo",
    ],
    alwayslink = 1,
)

# LINT.IfChange(legalize_tf_xla_call_module_to_stablehlo_pass)
cc_library(
    name = "legalize_tf_xla_call_module_to_stablehlo_pass",
    srcs = [
        "transforms/legalize_tf_xla_call_module_to_stablehlo_pass.cc",
    ],
    hdrs = [
        "transforms/legalize_tf_xla_call_module_to_stablehlo_pass.h",
    ],
    compatible_with = get_compatible_with_portable(),
    copts = [
        "-Ithird_party",
    ],
    deps = [
        "//tensorflow/compiler/mlir/tensorflow",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/strings:string_view",
        "@llvm-project//llvm:Support",
        "@llvm-project//mlir:FuncDialect",
        "@llvm-project//mlir:IR",
        "@llvm-project//mlir:Pass",
        "@llvm-project//mlir:QuantOps",
        "@llvm-project//mlir:ShapeDialect",
        "@llvm-project//mlir:Support",
        "@llvm-project//mlir:TransformUtils",
        "@llvm-project//mlir:Transforms",
        "@stablehlo//:stablehlo_ops",
        "@stablehlo//:stablehlo_serialization",
        "@stablehlo//:vhlo_ops",
    ],
    alwayslink = 1,
)
# LINT.ThenChange(//tensorflow/compiler/mlir/lite/stablehlo:legalize_tf_xla_call_module_to_stablehlo_pass)

cc_library(
    name = "fuse_convolution_pass",
    srcs = [
        "transforms/mhlo_passes/fuse_convolution_pass.cc",
    ],
    hdrs = [
        "transforms/stablehlo_passes.h",
    ],
    compatible_with = get_compatible_with_portable(),
    copts = [
        "-Ithird_party",
    ],
    deps = [
        "//tensorflow/compiler/mlir/lite:validators",
        "//tensorflow/compiler/mlir/quantization/common:attrs_and_constraints",
        "@llvm-project//llvm:Support",
        "@llvm-project//mlir:Dialect",
        "@llvm-project//mlir:FuncDialect",
        "@llvm-project//mlir:IR",
        "@llvm-project//mlir:Pass",
        "@llvm-project//mlir:ShapeDialect",
        "@llvm-project//mlir:Support",
        "@llvm-project//mlir:TransformUtils",
        "@llvm-project//mlir:Transforms",
        "@local_xla//xla/mlir_hlo",
    ],
    alwayslink = 1,
)

cc_library(
    name = "unfuse_batch_norm_pass",
    srcs = [
        "transforms/mhlo_passes/unfuse_batch_norm_pass.cc",
    ],
    hdrs = [
        "transforms/stablehlo_passes.h",
    ],
    compatible_with = get_compatible_with_portable(),
    copts = [
        "-Ithird_party",
    ],
    deps = [
        "//tensorflow/compiler/mlir/tensorflow:tensorflow_ops",
        "//tensorflow/compiler/mlir/tensorflow:tensorflow_types",
        "@llvm-project//llvm:Support",
        "@llvm-project//mlir:FuncDialect",
        "@llvm-project//mlir:IR",
        "@llvm-project//mlir:Pass",
        "@llvm-project//mlir:ShapeDialect",
        "@llvm-project//mlir:Support",
        "@llvm-project//mlir:TransformUtils",
        "@local_xla//xla/mlir_hlo",
    ],
    alwayslink = 1,
)

cc_library(
    name = "rename_entrypoint_to_main",
    srcs = [
        "transforms/rename_entrypoint_to_main.cc",
    ],
    hdrs = [
        "transforms/rename_entrypoint_to_main.h",
    ],
    compatible_with = get_compatible_with_portable(),
    copts = [
        "-Ithird_party",
    ],
    deps = [
        "//tensorflow/compiler/mlir/tensorflow",
        "@llvm-project//llvm:Support",
        "@llvm-project//mlir:FuncDialect",
        "@llvm-project//mlir:IR",
        "@llvm-project//mlir:Pass",
        "@llvm-project//mlir:Support",
    ],
    alwayslink = 1,
)
