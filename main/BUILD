cc_library(
    name = "mathlib",
    srcs = ["mathlib.cpp"],
    hdrs = ["mathlib.h"],
)

cc_binary(
    name = "fuzz_target_math",
    srcs = ["fuzz_target_math.cpp"],
    deps = [":mathlib"],  # Länka mot mathlib
    copts = ["-fsanitize=fuzzer", "-g", "-O1", "-fno-omit-frame-pointer"],
    linkopts = ["-fsanitize=fuzzer", "-lstdc++"],
)




#    linkopts = ["-lstdc++"],
