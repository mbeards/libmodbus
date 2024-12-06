load("@bazel_skylib//rules:expand_template.bzl", "expand_template")

cc_library(
    name = "modbus",
    srcs = glob(["src/*.c"]) + [
        ":generate_config_h",
        ":generate_modbus_version_h",
    ],
    hdrs = glob(["src/*.h"]),
    copts = ["-DHAVE_CONFIG_H"],
    includes = ["src"],
)

cc_binary(
    name = "libmodbus.so.0",
    srcs = [
    ],
    linkopts = [
        "-Wl,-soname,libmodbus.so.0",
        # Linker script to hide non-modbus symbols.
        "-Wl,--version-script",
        "$(location modbus-versionscript.lds)",
    ],
    linkshared = 1,
    deps = [
        ":modbus",
        ":modbus-versionscript.lds",
    ],
)

cc_library(
    name = "modbus_shared",
    srcs = [
        "libmodbus.so.0",
    ],
    hdrs = [
        "config.h",
        "src/modbus.h",
        "src/modbus-rtu.h",
        "src/modbus-tcp.h",
        "src/modbus-version.h",
    ],
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

genrule(
    name = "generate_config_h",
    outs = ["src/config.h"],
    cmd = """cat > $@ <<EOF
#define HAVE_STRLCPY 1
#define HAVE_DECL_TIOCM_RTS 1
#define HAVE_DECL_TIOCSRS485 1
#undef HAVE_ACCEPT4
#undef HAVE_GAI_STRERROR
#define HAVE_NETINET_IN_H 1
#define HAVE_NETINET_IP_H 1
""",
)