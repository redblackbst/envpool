load("@pip_requirements//:requirements.bzl", "requirement")

filegroup(
    name = "clang_tidy_config",
    data = [".clang-tidy"],
)

py_binary(
    name = "setup",
    srcs = [
        "setup.py",
    ],
    data = [
        "README.md",
        "setup.cfg",
        "//envpool",
    ],
    main = "setup.py",
    python_version = "PY3",
    deps = [
        requirement("setuptools"),
        requirement("wheel"),
    ],
)

load("@rules_python//python:packaging.bzl", "py_package", "py_wheel", "py_wheel_dist")

py_package(
    name = "pkg",
    packages = ["envpool"],
    deps = ["//envpool:envpool"],
)

config_setting(
    name = "cfg_py312",
    flag_values = {"@rules_python//python/config_settings:python_version": "3.12"},
)

config_setting(
    name = "cfg_py313",
    flag_values = {"@rules_python//python/config_settings:python_version": "3.13"},
)

config_setting(
    name = "cfg_py314",
    flag_values = {"@rules_python//python/config_settings:python_version": "3.14"},
)

config_setting(
    name = "cfg_py314t",
    flag_values = {
        "@rules_python//python/config_settings:python_version": "3.14",
        "@rules_python//python/config_settings:py_freethreaded": "yes",
    },
)

ENVPOOL_VERSION = "0.8.4"
PLAT = "linux_x86_64"

py_wheel(
    name = "wheel",
    distribution = "envpool",
    version = ENVPOOL_VERSION,
    platform = PLAT,
    python_tag = select({
        ":cfg_py312": "cp312",
        ":cfg_py313": "cp313",
        ":cfg_py314": "cp314",
        ":cfg_py314t": "cp314",
        "//conditions:default": "cp312",
    }),
    abi = select({
        ":cfg_py312": "cp312",
        ":cfg_py313": "cp313",
        ":cfg_py314": "cp314",
        ":cfg_py314t": "cp314t",
        "//conditions:default": "cp312",
    }),
    python_requires = select({
        ":cfg_py312": ">=3.12,<3.13",
        ":cfg_py313": ">=3.13,<3.14",
        ":cfg_py314": ">=3.14,<3.15",
        ":cfg_py314t": ">=3.14,<3.15",
        "//conditions:default": ">=3.12,<3.13",
    }),
    deps = [":pkg"],
    requires_file = "//third_party/pip_requirements:requirements-release.txt",
)

py_wheel_dist(
    name = "wheel_dist",
    wheel = ":wheel",
    out = "dist",
)
