We enforce certain code formatting and prevent other stylistic issues automatically.

## Overview

You can check locally by running
```
./contrib/utilities/indent
```
in your main deal.ii directory or ``make indent`` from inside your build directory.

This script will apply formatting automatically and report other potential problems in your contribution. It is recommended to run this locally before you create a pull request (see https://github.com/dealii/dealii/wiki/Contributing for details about that). Otherwise, the continuous integration tester, that also runs the same script, will flag your pull request.

Currently, the indent script does the following things:
1. Format all header, source, and .inst.in files using clang-format.
2. Fix permissions and line endings (for example remove executable bits for source files).
3. Check for valid authorship of your commits (see below).

