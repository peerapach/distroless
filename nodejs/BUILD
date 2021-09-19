package(default_visibility = ["//visibility:public"])

load("@io_bazel_rules_docker//container:container.bzl", "container_image")
load("@io_bazel_rules_docker//contrib:test.bzl", "container_test")
load("//base:distro.bzl", "DISTRO_SUFFIXES")
load("//:checksums.bzl", ARCHITECTURES = "BASE_ARCHITECTURES")

NODEJS_MAJOR_VERISONS = ("12", "14", "16")

[
    container_image(
        name = "nodejs" + major_version + ("" if (not mode) else mode) + "_" + arch + distro_suffix,
        architecture = arch,
        base = ("//cc:cc" if (not ("debug" in mode)) else "//cc:debug") + "_root_" + arch + distro_suffix,
        entrypoint = ["/nodejs/bin/node"],
        tars = ["@nodejs" + major_version + "_" + arch + "//:tar"],
    )
    for mode in [
        "",
        "_debug",
    ]
    for major_version in NODEJS_MAJOR_VERISONS
    for arch in ARCHITECTURES
    for distro_suffix in DISTRO_SUFFIXES + ["_debian11"]
]

[
    container_test(
        name = "nodejs" + major_version + ("" if (not mode) else mode) + "_" + arch + distro_suffix + "_test",
        configs = ["testdata/nodejs" + major_version + ".yaml"],
        image = "nodejs" + major_version + ("" if (not mode) else mode) + "_" + arch + distro_suffix,
        tags = [
            arch,
            "manual",
        ],
    )
    for mode in [
        "",
        "_debug",
    ]
    for major_version in NODEJS_MAJOR_VERISONS
    for arch in ARCHITECTURES
    for distro_suffix in DISTRO_SUFFIXES + ["_debian11"]
]
