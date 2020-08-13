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

## Commit authorship

We require that all the commits are made with a valid name ("Firstname Lastname") and a valid email address. This is mainly to be able to attribute contributions to you for each release and to be able to track the number of contributors over time.
You can check your setting using:
```
$ git config --global -l
user.name=John Doe
user.email=john.doe@gmail.com
```
For more details on how to change your username or email and how to fix older commits, please see the excellent guide at https://www.git-tower.com/learn/git/faq/change-author-name-email

A short list of instructions to update an open pull request with more than one commit: https://www.deployhq.com/git/faqs/update-author-committer-multiple-git-commits

For main developers: If commits or email addresses need to be modified after they are merged to master, see https://github.com/dealii/dealii/blob/master/.mailmap