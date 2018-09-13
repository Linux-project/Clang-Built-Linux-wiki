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
$ ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- make CC=clang
```

Note that unlike gcc/binutils, clang ships with support for all backends by default (you can configure them off, but this is considered an antipattern).  `ARCH=` specifies the kernel's notion of target ISA. These can be found in the kernel sources under the `arch/` directory.  `CROSS_COMPILE` is a prefix for binutils tools.  KBUILD will literally run `$ $CROSS_COMPILE-as` and `$ $CROSS_COMPILE-ld`, so verify that you've installed the correct cross version of binutils and they're accessible from your `$PATH`.