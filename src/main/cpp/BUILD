# Description:
#   The Bazel launcher.
package(
    default_visibility = ["//visibility:public"],
)

WIN_LINK_OPTS = [
    "-DEFAULTLIB:advapi32.lib",  # GetUserNameW
    "-DEFAULTLIB:ole32.lib",  # CoTaskMemFree
    "-DEFAULTLIB:shell32.lib",  # SHGetKnownFolderPath
    "-DEFAULTLIB:ws2_32.lib",  # grpc
]

cc_library(
    name = "blaze_util",
    srcs = [
        "blaze_util.cc",
        "global_variables.h",
        "startup_options.h",
    ] + select({
        "//src/conditions:darwin": [
            "blaze_util_darwin.cc",
            "blaze_util_posix.cc",
        ],
        "//src/conditions:darwin_x86_64": [
            "blaze_util_darwin.cc",
            "blaze_util_posix.cc",
        ],
        "//src/conditions:freebsd": [
            "blaze_util_freebsd.cc",
            "blaze_util_posix.cc",
        ],
        "//src/conditions:windows": [
            "blaze_util_windows.cc",
        ],
        "//src/conditions:windows_msys": [
            "blaze_util_windows.cc",
        ],
        "//src/conditions:windows_msvc": [
            "blaze_util_windows.cc",
        ],
        "//conditions:default": [
            "blaze_util_linux.cc",
            "blaze_util_posix.cc",
        ],
    }),
    hdrs = [
        "blaze_util.h",
        "blaze_util_platform.h",
    ],
    linkopts = select({
        "//src/conditions:darwin": [
            "-framework CoreFoundation",
        ],
        "//src/conditions:darwin_x86_64": [
            "-framework CoreFoundation",
        ],
        "//src/conditions:freebsd": [
        ],
        "//src/conditions:windows": WIN_LINK_OPTS,
        "//src/conditions:windows_msvc": WIN_LINK_OPTS,
        "//conditions:default": [
            "-lrt",
        ],
    }),
    deps = [
        "//src/main/cpp/util",
        "//src/main/cpp/util:blaze_exit_code",
    ] + select({
        "//src/conditions:windows": ["//src/main/native/windows:lib-file"],
        "//src/conditions:windows_msys": ["//src/main/native/windows:lib-file"],
        "//src/conditions:windows_msvc": ["//src/main/native/windows:lib-file"],
        "//conditions:default": [],
    }),
)

cc_binary(
    name = "client",
    srcs = [
        "blaze.cc",
        "blaze.h",
        "global_variables.cc",
        "global_variables.h",
        "main.cc",
    ] + select({
        "//src/conditions:windows": ["//src/main/native/windows:resources.o"],
        "//src/conditions:windows_msvc": ["//src/main/native/windows:resources.o"],
        "//conditions:default": [],
    }),
    copts = select({
        "//src/conditions:windows": ["/wd4018"],
        "//src/conditions:windows_msvc": ["/wd4018"],
        "//conditions:default": ["-Wno-sign-compare"],
    }),
    linkopts = select({
        "//src/conditions:darwin": [
        ],
        "//src/conditions:darwin_x86_64": [
        ],
        "//src/conditions:freebsd": [
            "-lprocstat",
            "-lm",
        ],
        "//src/conditions:windows_msvc": [
        ],
        "//conditions:default": [
            "-lrt",
            "-ldl",
        ],
    }),
    visibility = ["//src:__pkg__"],
    deps = [
        ":blaze_util",
        ":option_processor",
        ":startup_options",
        ":workspace_layout",
        "//src/main/cpp/util",
        "//src/main/cpp/util:errors",
        "//src/main/cpp/util:logging",
        "//src/main/cpp/util:strings",
        "//src/main/protobuf:command_server_cc_proto",
        "//third_party/ijar:zip",
    ],
)

cc_library(
    name = "option_processor",
    srcs = ["option_processor.cc"],
    hdrs = [
        "option_processor.h",
        "option_processor-internal.h",
    ],
    visibility = [
        "//src:__pkg__",
        "//src/test/cpp:__pkg__",
    ],
    deps = [
        ":blaze_util",
        ":startup_options",
        ":workspace_layout",
        "//src/main/cpp/util",
        "//src/main/cpp/util:blaze_exit_code",
        "//src/main/cpp/util:logging",
    ],
)

cc_library(
    name = "startup_options",
    srcs = ["startup_options.cc"],
    hdrs = ["startup_options.h"],
    visibility = [
        "//src:__pkg__",
        "//src/test/cpp:__pkg__",
    ],
    deps = [
        ":blaze_util",
        ":workspace_layout",
        "//src/main/cpp/util",
        "//src/main/cpp/util:blaze_exit_code",
        "//src/main/cpp/util:errors",
    ],
)

cc_library(
    name = "workspace_layout",
    srcs = ["workspace_layout.cc"],
    hdrs = ["workspace_layout.h"],
    visibility = [
        "//src:__pkg__",
        "//src/test/cpp:__pkg__",
    ],
    deps = [
        ":blaze_util",
        "//src/main/cpp/util",
    ],
)

filegroup(
    name = "srcs",
    srcs = glob(["**"]) + ["//src/main/cpp/util:srcs"],
    visibility = ["//src:__pkg__"],
)
