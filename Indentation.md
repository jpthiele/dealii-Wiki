We enforce certain code formatting and prevent other stylistic issues automatically.

## Overview

deal.II uses clang-format 11.1 to normalize indentation. A style file is provided at
```bash
${SOURCE_DIR}/.clang-format
```

#### clang-format -i

Before a commit, you could manually run
```bash
clang-format -i <file>
```
on each of the files you have edited or changed in other ways. This will make sure indentation is conforming to the style guidelines outlined in this page.

#### make indent

The manual process described above is cumbersome. Consequently, and more easily, you can just run
```bash
make indent
```
from inside your build directory, to indent all source files that have been changed recently. 
If the system you are working on has more than one version of clang-format installed (or if it is not in the path) you should replace the above make indent command with
```bash
make DEAL_II_CLANG_FORMAT=/path/to/clang-11.1/clang-format indent
```
to point to the correct executable. 

#### contrib/utilities/indent

Alternatively, for example if you don't have a build directory (maybe because you just wanted to change a few lines of the documentation, rather than build a complete version of deal.II), you can also just run the following command in the source directory:
```
./contrib/utilities/indent
```
This is equivalent to running `make indent` in the build directory.

This script will apply formatting automatically and report other potential problems in your contribution. It is recommended to run this locally before you create a pull request (see https://github.com/dealii/dealii/wiki/Contributing for details about that). Otherwise, the continuous integration tester, that also runs the same script, will flag your pull request.

Currently, the indent script does the following things:
1. Format all header, source, and .inst.in files using clang-format.
2. Fix permissions and line endings (for example remove executable bits for source files).
3. Check for valid authorship of your commits (see below).

#### pre-commit hook

If you want to make sure that the indenting is correct for all your commits, you might want to set up a pre-commit hook. One way to do so, is to copy ${SOURCE_DIR}/contrib/git-hooks/pre-commit to ${SOURCE_DIR}/.git/hooks/pre-commit and make sure it is executable.


