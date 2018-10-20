1. Add this as a remote from a kernel checkout.
```sh
$ git remote add --track master clang-built-linux git@github.com:ClangBuiltLinux/linux.git
$ git fetch clang-built-linux
$ git remote -v
...
clang-built-linux	git@github.com:ClangBuiltLinux/linux.git (fetch)
clang-built-linux	git@github.com:ClangBuiltLinux/linux.git (push)
```
2. Pull from torvalds/linux
3. Push to clang-built-linux/master

```sh
$ git push clang-built-linux
```

Do not force push to master.  If there's some kind of conflict, you likely have done something wrong.