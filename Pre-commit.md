This documents the use and setup of `pre-commit` 
as a tool for locally running some quick checks on each commit.

# What it does
The tool is used to set up a so-called git hook, 
which is practically a program call inserted into the sequence of steps
performed by `git` on each call of `git commit`.
In this specific case, 
the hook runs a few checks on all files in the staging area, 
before the actual commit step. 
If any checks fail, the commit will not be added to the history.

Let's motivate this by an example.
Say you forgot to format a C++ source file in a pull request.
Without `pre-commit` you will get a notification 
once the `indent` CI check has failed on GitHub. 
But at that point you might already be working on something else,
so you will have to switch contexts again to fix the formatting issue in your PR.
With `pre-commit` you will be notified quite immediately during `git commit`
so you can fix the issue right away. 

For an extensive documentation
see the [pre-commit project page](https://pre-commit.com/)

# How to set up

## Installation of the tool
Depending on your Python setup/preference, 
you can either install the tool through a python package manager, 
e.g. `pip` or `conda`, or through your system package manager, e.g. `apt` or `dnf`.  

## Manual usage
At this point you can already run the tool in any repo with a valid config file 
(`.pre-commit-config.yml`) by typing `pre-commit run` in your console/terminal.
This will run the configured checks on all files in the git staging area 
,i.e. everything under 'Changes to be commited' shown after `git status`.

You can also run the checks on all files in the repository by calling
`pre-commit run --all-files`

## Installation of the hook
To automatically execute `pre-commit run` during `git commit` 
call `pre-commit install` once per repository where you want to enable this.

# What it checks
Since some check names can be cryptic we will quickly go over what each check does 

1. `check for added large files` ensures that no files above 8MB end up in the repository. (Note: limit can be increased if necessary)
2. `check for merge conflicts` ensures that this commit does not introduce a merge conflict by not being up to date with the remote branch.
3. `check json/toml/yaml` all these check indentation and syntax of JSON, TOML, and YAML files respectively.
6. `fix end of files` ensures that all files end in a single newline, i.e. no additional empty lines.
7. `don't commit to branch` ensures that you don't accidently commit to the master branch of your fork. (Which makes it easier to stay up to date and to squash commits in pull requests.)
8. `trim trailing whitespace` ensures that all lines in text files end without any spaces.
9. `Detect hardcoded secrets` checks to see if any passwords or similar are stored openly in a file.
10. `clang-format` runs clang-format on all C++ source files 
to ensure they meet the format specifications of deal.II
11. `typos` runs a spell checker on text files (including source files).
