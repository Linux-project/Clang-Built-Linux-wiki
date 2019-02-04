If you would like to build Clang (and lld) from source for testing purposes, it is really easy! You should have `cmake` and `ninja` installed from your distribution.

```
$ git clone git://github.com/llvm/llvm-project
$ cd llvm-project
$ mkdir build
$ cd build
$ cmake -G Ninja \
        -DCMAKE_BUILD_TYPE=Release \
        -DLLVM_ENABLE_PROJECTS="clang;lld" \
        -DLLVM_ENABLE_WARNINGS=OFF \
        ../llvm
$ ninja
```

If you want Clang to compile faster and the time it takes to compile other projects to potentially decrease, you can run the following command, which:
* Uses `clang` if it is installed to initially build Clang, falling back to `gcc` if it isn't present.
* Uses `-march=native -mtune=native` to generate code optimized for your current processor.
* Turns off a lot of unnecessary features like tests, examples, and other things that the kernel doesn't care about.
* Only enables the architectures that we are currently testing (aarch64, arm32, powerpc, x86).
* Uses `ld.lld` for linking (which has been shown to be MUCH faster than `ld.bfd` and slightly faster than `ld.gold`), falling back to `ld.gold` then `ld.bfd` if it isn't present.

```
cmake -Wno-dev \
      -G Ninja \
      -DCLANG_ENABLE_ARCMT=OFF \
      -DCLANG_ENABLE_STATIC_ANALYZER=OFF \
      -DCLANG_PLUGIN_SUPPORT=OFF \
      -DCMAKE_BUILD_TYPE=Release \
      -DCMAKE_C_COMPILER="$(command -v clang || command -v gcc)" \
      -DCMAKE_C_FLAGS="-O2 -march=native -mtune=native" \
      -DCMAKE_CXX_COMPILER="$(command -v clang++ || command -v g++)" \
      -DCMAKE_CXX_FLAGS="-O2 -march=native -mtune=native" \
      -DLLVM_ENABLE_BINDINGS=OFF \
      -DLLVM_EXTERNAL_CLANG_TOOLS_EXTRA_SOURCE_DIR="" \
      -DLLVM_ENABLE_PROJECTS="clang;lld" \
      -DLLVM_CCACHE_BUILD=ON \
      -DLLVM_ENABLE_OCAMLDOC=OFF \
      -DLLVM_ENABLE_TERMINFO=OFF \
      -DLLVM_ENABLE_WARNINGS=OFF \
      -DLLVM_INCLUDE_EXAMPLES=OFF \
      -DLLVM_INCLUDE_TESTS=OFF \
      -DLLVM_INCLUDE_DOCS=OFF \
      -DLLVM_TARGETS_TO_BUILD="AArch64;ARM;PowerPC;X86" \
      -DLLVM_USE_LINKER="$(for LD in lld gold bfd; do command -v ld.${LD} &>/dev/null && break; done; echo ${LD})" \
      ../llvm
```

Once it is done, you can `export PATH=${PWD}/bin:${PATH}` to have it available for kernel compiles.