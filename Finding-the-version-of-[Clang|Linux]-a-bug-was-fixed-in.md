# Linux

Given `sha` of commit, we want to know the first tag that contains the fix.
```sh
tag=$1
git describe --contains "$tag" | sed 's/~.*//'
```

# Clang

eh, depends on the r123456 svn-revision, which is not easy to map to a release version, and if it got cherry picked back to a bugfix release. WIP.