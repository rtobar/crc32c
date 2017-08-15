# CRC32C

[![Build Status](https://travis-ci.org/google/crc32c.svg?branch=master)](https://travis-ci.org/google/crc32c)
[![Build Status](https://ci.appveyor.com/api/projects/status/moiq7331pett4xuj/branch/master?svg=true)](https://ci.appveyor.com/project/pwnall/crc32c)

**This is not an official Google product.**
This is code extracted from [LevelDB](https://github.com/google/leveldb), which
is a stable key-value store that is widely used at Google.

This project collects a few CRC32C implementations under an umbrella that
dispatches to a suitable implementation based on the host computer's hardware
capabilities.

CRC32C is specified as the CRC that uses the iSCSI polynomial in
[RFC 3720](https://tools.ietf.org/html/rfc3720#section-12.1). The polynomial was
introduced by G. Castagnoli, S. Braeuer and M. Herrmann. CRC32C is used in
software such as Btrfs, ext4, Ceph and leveldb.


## Prerequisites

This project uses [CMake](https://cmake.org/) for building and testing. CMake is
available in all popular Linux distributions, as well as in
[Homebrew](https://brew.sh/).

This project uses submodules for dependency management.

```bash
git submodule update --init --recursive
```

If you're using [Atom](https://atom.io/), the following packages can help.

```bash
apm install autocomplete-clang build build-cmake clang-format language-cmake \
    linter linter-clang
```

If you don't mind more setup in return for more speed, replace
`autocomplete-clang` and `linter-clang` with `you-complete-me`. This requires
[setting up ycmd](https://github.com/Valloric/ycmd#building).

```bash
apm install autocomplete-plus build build-cmake clang-format language-cmake \
    linter you-complete-me
```

## Building

The following commands build the project.

```bash
mkdir out
cd out
cmake .. && cmake --build .
```

## Development

The following command (when executed from `out/`) (re)builds the project and
runs the tests.

```bash
cmake .. && cmake --build . && ctest --output-on-failure
```


### Android testing

The following command builds the project against the Android NDK, which is
useful for benchmarking against ARM processors.

```bash
cmake .. -DCMAKE_SYSTEM_NAME=Android \
    -DCMAKE_ANDROID_NDK=$HOME/Library/Android/sdk/ndk-bundle \
    -DCMAKE_ANDROID_ARCH_ABI=arm64-v8a \
    -DCMAKE_ANDROID_NDK_TOOLCHAIN_VERSION=clang -DCMAKE_BUILD_TYPE=Release \
    -DRUN_HAVE_POSIX_REGEX=0 -DCRC32C_USE_GLOG=0 && \
    cmake --build .
```

The following commands install and run the benchmarks.

```bash
adb push crc32c_bench /data/local/tmp
adb shell chmod +x /data/local/tmp/crc32c_bench
adb shell 'cd /data/local/tmp && ./crc32c_bench'
adb shell rm /data/local/tmp/crc32c_bench
```

The following commands install and run the tests.

```bash
adb push crc32c_tests /data/local/tmp
adb shell chmod +x /data/local/tmp/crc32c_tests
adb shell 'cd /data/local/tmp && ./crc32c_tests'
adb shell rm /data/local/tmp/crc32c_tests
```
