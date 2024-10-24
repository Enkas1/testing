# Commands
bazel build //main:fuzz_target_math --extra_toolchains=//toolchain2:cc_toolchain_for_linux_x86_64_afl  # Behövs endast extra toolchain här för att vi har två olika

readelf --string-dump=.comment fuzz_target_math            # Ändra namnet på den binära filen!

afl-fuzz -i input_corpus -o fuzz_output -- bazel-bin/main/fuzz_target_math           # Fuzza target

readelf -s hello-world | grep __afl                         # Se om det finns afl filer efter kompilering


# BUG?!
https://www.google.com/search?client=ubuntu-sn&channel=fs&q=%5B-%5D+PROGRAM+ABORT+%3A+GCC+and+plugin+have+incompatible+versions%2C+expected+GCC+13.2.0%2C+is+13.2.0++++++++++Location+%3A+plugin_init%28%29%2C+instrumentation%2Fafl-gcc-pass.so.cc%3A482

# Teknisk skuld: 
Tweekat tool_path(name="gcc", path="/usr/bin/afl-clang-fast++"), 