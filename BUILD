licenses(["notice"])

exports_files([
    "LICENSE",
    "NOTICE",
])

COPTS = []

LIBS = [
    "@libprim//:prim",
    "@librnd//:rnd",
]

cc_library(
    name = "lib",
    srcs = glob(
        ["src/**/*.cc"],
        exclude = [
            "src/main.cc",
            "src/**/*_TEST*",
        ],
    ),
    hdrs = glob(
        [
            "src/**/*.h",
            "src/**/*.tcc",
        ],
        exclude = ["src/**/*_TEST*"],
    ),
    copts = COPTS,
    includes = [
        "src",
    ],
    visibility = ["//visibility:private"],
    deps = LIBS,
    alwayslink = 1,
)

cc_binary(
    name = "rndex",
    srcs = ["src/main.cc"],
    copts = COPTS,
    includes = [
        "src",
    ],
    visibility = ["//visibility:public"],
    deps = [
        ":lib",
    ] + LIBS,
)

cc_library(
    name = "test_lib",
    testonly = 1,
    srcs = glob([
        "src/**/*_TEST*.cc",
    ]),
    hdrs = glob([
        "src/**/*_TEST*.h",
        "src/**/*_TEST*.tcc",
    ]),
    copts = COPTS,
    visibility = ["//visibility:private"],
    deps = [
        ":lib",
        "@googletest//:gtest_main",
    ] + LIBS,
    alwayslink = 1,
)

cc_test(
    name = "rndex_test",
    args = [
        "--gtest_color=yes",
    ],
    copts = COPTS,
    visibility = ["//visibility:public"],
    deps = [
        ":test_lib",
    ] + LIBS,
)

genrule(
    name = "lint",
    srcs = glob([
        "src/**/*.cc",
    ]) + glob([
        "src/**/*.h",
        "src/**/*.tcc",
    ]),
    outs = ["linted"],
    cmd = """
    python $(location @cpplint//:cpplint) \
      --root=$$(pwd)/src \
      --headers=h,tcc \
      --extensions=cc,h,tcc \
      --quiet $(SRCS) > $@
    echo // $$(date) > $@
  """,
    tools = [
        "@cpplint",
    ],
    visibility = ["//visibility:public"],
)
