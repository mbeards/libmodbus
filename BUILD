load("@bazel_skylib//rules:expand_template.bzl", "expand_template")

cc_library(
    name = "libmodbus",
    srcs = glob(["src/*.c"]) + [":generate_modbus_version_h"],
    hdrs = glob(["src/*.h"]),
    includes = ["src"],
)

expand_template(
    name = "generate_modbus_version_h",
    out = "src/modbus-version.h",
    substitutions = {
        "@LIBMODBUS_VERSION_MAJOR@": "1",
        "@LIBMODBUS_VERSION_MINOR@": "2",
        "@LIBMODBUS_VERSION_MICRO@": "3",
        "@LIBMODBUS_VERSION@": "1.2.3",
    },
    template = "src/modbus-version.h.in",
)
