workspace(name = "papi_test")
load("@bazel_tools//tools/build_defs/repo:git.bzl", "git_repository", "new_git_repository")

git_repository(
    name = "com_google_absl",
    remote = "https://github.com/abseil/abseil-cpp.git",
    commit = "aa844899c937bde5d2b24f276b59997e5b668bde",
)

git_repository(
    name = "com_google_googletest",
    remote = "https://github.com/google/googletest.git",
    commit = "703bd9caab50b139428cea1aaff9974ebee5742e",
)

git_repository(
    name = "com_google_benchmark",
    remote = "https://github.com/google/benchmark.git",
    commit = "090faecb454fbd6e6e17a75ef8146acb037118d4",
)

PAPI_GENRULE_BUILD = """
package(default_visibility = ["//visibility:public"]) 

genrule(
    name = "papi_genrule",
    srcs = glob(["**"]),
    outs = ["libpapi.a", "papi_internal.h", "papiStdEventDefs.h"],
    cmd = " && ".join([
        "cd external/edu_utk_papi/src/",
        "./configure --with-static-lib=yes",
        "make",
        "cd ../../../",
        "cp external/edu_utk_papi/src/libpapi.a $(location libpapi.a)",
        "cp external/edu_utk_papi/src/papi.h $(location papi_internal.h)",
        "cp external/edu_utk_papi/src/papiStdEventDefs.h $(location papiStdEventDefs.h)",
    ]),
)

cc_library(
    name = "papi",
    srcs = ["libpapi.a"],
    hdrs = ["papi_internal.h", "papiStdEventDefs.h"],
    alwayslink = 1,
)
"""

new_git_repository(
    name = "edu_utk_papi",
    remote = "https://bitbucket.org/icl/papi",
    commit = "1a0bf73d8be1cc4f5cab316b8e40dbd57d507149",
    shallow_since = "1583285722 +0000",
    build_file_content = PAPI_GENRULE_BUILD,
)
