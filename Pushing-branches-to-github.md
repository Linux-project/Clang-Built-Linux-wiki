# Add remote

Git is a distributed version control system; there is no master copy of the code other than what people provide consensus on.  As such, you can add many "remotes" (git servers) and push to whichever one you want.

```sh
$ cd <existing linux kernel git checkout>
$ git remote add github git@github.com:ClangBuiltLinux/linux.git
$ git fetch github

$ cd <existing llvm git checkout>
$ git remote add github git remote add github git@github.com:ClangBuiltLinux/llvm.git
$ git fetch github

$ cd <existing clang git checkout>
$ git remote add github git@github.com:ClangBuiltLinux/clang.git
$ git fetch github
```

You can check your list of remotes with `git remote -v`.  Typically, your original checkout's remote is named `origin` unless you give it a different name during checkout or rename it later.  When you `git push` code, you can push to an explicit remote via `git push <remote name>` (in this case, we'd want `git push github`), where `origin` is the implied default branch if unspecified.

# Workflow

Adding the remote is a one and done routine for a given git repository checkout.  For each set of commits, you SHOULD ALWAYS work out of a new branch.  Branches are free and cheap to create, and most importantly, you can abandon them easily if they don't work out.  Don't commit new features to the master branch (most github projects lock this branch).  The typical workflow should look like:

```sh
$ git checkout -b <helpful branch name about the feature so you remember in a few months what commits are here>
$ <make changes>
$ git commit <options/files>
$ <repeat above 2>
$ git push github
```

If the `master` branch is progressing, it's a good idea to `git pull --rebase master` before pushing (otherwise your branch is out of date, which is not bad per se).  It's ok to push multiple commits, commits in git can be squashed into one later.