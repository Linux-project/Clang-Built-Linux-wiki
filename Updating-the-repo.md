# TODO: incomplete

1. Add this as a remote from a kernel checkout.
```sh
$ git remote add --track master clang-built-linux git@github.com:ClangBuiltLinux/linux.git
$ git remote -v
...
clang-built-linux	git@github.com:ClangBuiltLinux/linux.git (fetch)
clang-built-linux	git@github.com:ClangBuiltLinux/linux.git (push)
```
2. Pull from torvalds/linux
3. Push to clang-built-linux/master

Do not force push to master.

### TODO: AUTOMATE (issue #1)