If you would like to build Clang (and lld) from source for testing purposes, it is really easy! You should have `cmake` and `ninja` installed from your distribution.

```
$ git clone git://github.com/llvm/llvm-project
$ cd llvm-project
$ mkdir build
$ cd build
$ cmake -G Ninja \
        -DCMAKE_BUILD_TYPE=Release \
        -DLLVM_ENABLE_PROJECTS="clang;lld;compiler-rt" \
        -DLLVM_ENABLE_WARNINGS=OFF \
        ../llvm
$ ninja
```

If you would like a slightly more optimized clang binary and quicker compile times, use our tc-build repo:

```
$ git clone git://github.com/ClangBuiltLinux/tc-build
$ cd tc-build
$ ./build-llvm.py
```

Please see [that repo's README](https://github.com/ClangBuiltLinux/tc-build/blob/master/README.md) for more information.