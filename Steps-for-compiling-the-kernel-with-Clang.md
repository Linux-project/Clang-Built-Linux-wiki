## Dependencies

We still defer to binutils for assembling and linking (though we happily accept bug reports from trying LLVM's equivalents).

For cross-compiling, you need to have the cross-target versions of binutils in your `$PATH`:
```
$ sudo apt install binutils-aarch64-linux-gnu
```

Note: The minimal version of Clang required to build the kernel is hard to exactly pin down, due to the large combinations of kernel [LTS] branch, target ISA, kernel configurations.  Test your combination, and report issues in our [bug tracker](https://github.com/ClangBuiltLinux/linux/issues).  Clang 4 was used to ship a Clang built arm64 kernel on the Google Pixel phone.

## Compiling for host

```
$ make CC=clang HOSTCC=clang
```

## Cross compiling

```
$ ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- make CC=clang HOSTCC=clang
```

Note that unlike gcc/binutils, clang ships with support for all backends by default (you can configure them off, but this is considered an antipattern).  `ARCH=` specifies the kernel's notion of target ISA. These can be found in the kernel sources under the `arch/` directory.  `CROSS_COMPILE` is a prefix for binutils tools.  KBUILD will literally run `$ $CROSS_COMPILE-as` and `$ $CROSS_COMPILE-ld`, so verify that you've installed the correct cross version of binutils and they're accessible from your `$PATH`.

## Relevant parts of Kbuild

[This is the relevant part](https://github.com/ClangBuiltLinux/linux/blob/2c45e7fbc962be1b03f2c2af817a76f5ba810af2/Makefile#L407-L416) of the Kernel's top level Makefile.  These define the internal variables that will be used during the build.  The tools are prefixed with `CROSS_COMPILE` if that's set in the environment.

For example, to substitute GNU `objcopy` for `llvm-objcopy`, you might invoke your kernel build as:
```
$ make OBJCOPY=llvm-objcopy
```
where `llvm-objcopy` was already accessible via `$PATH`.  Or when cross compiling:
```
$ ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- make OBJCOPY=llvm-objcopy
```

You can see the relevant variables being overridden in our CI scripts [here](https://github.com/ClangBuiltLinux/continuous-integration/blob/dc0a4140bf305f2aa21606e62432afd4d91c662e/driver.sh#L227-L228).

So the list of relevant variables is `CC=clang`, `AS=clang`, `LD=ld.lld`, `AR=llvm-ar`, `NM=llvm-nm`, `STRIP=llvm-strip`, `OBJCOPY=llvm-objcopy`, `OBJDUMP=llvm-objdump`, `HOSTCC=clang`, `HOSTLD=ld.lld`, and `HOSTAR=llvm-ar`.  (TODO: It's probably worthwhile to simplify this upstream).

## Goals

The eventual goal is to be able to compile the kernel hermetically w/ only Clang and LLVM tools.