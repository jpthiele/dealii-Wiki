# Contributing code via Git and GitHub

deal.II is developed using [Git](https://git-scm.com) as a version control tool, [GitHub](https://github.com/about) as the central source-code host and a [branch-based workflow](https://guides.github.com/introduction/flow/). To contribute a patch back to the project (which we greatly appreciate), the following summarises the steps that you need to take to do this:

1. Create a GitHub account and _fork_ the [deal.II project](https://github.com/dealii/dealii).
2. Make a local _clone_ of your fork. This will provide you a local copy of your fork of deal.II, and will configure the local Git project such that the you can _push_ your changes _upstream_ to your _remote_ version of deal.II as hosted on GitHub. The name traditionally given to this remote repository is `origin`.
3. Create a new feature branch on which you will be making changes. This marks the point from which your copy of the project differs from that of the main development branch of the parent repository (that's ours). Since we do our development in a branch called `master`, you'll want to use this `master` branch as the source of your branch. Better yet, if you choose `origin/master` as the branch source, then not only will Git use `master` as the version on which to _base_ your new feature branch, but will also configure the branch such that the changes that you make can be _pushed_ upstream to your fork of the repository.
4. Once your made changes to the project (by modifying existing files or creating new ones) you _commit_ each change. With this commit, Git records the set of differences between your updated view of the project and the previous one. With each commit you can write a short message describing what your particular set of changes does. We typically write a very short summary in the first line of the commit message. If necessary this is followed by a longer description from the third line onwards.
    - We have somewhat strict guidelines as to the formatting of all submitted code, and we test that the patches that are submitted conform to these guidelines. In order to satisfy the tester that automatically checks the code formatting, we recommend that you first run the script found at `<path/to/deal.II/fork>/contrib/utilities/indent.sh` before committing your changes.
    - If you submit a patch that adds a new feature or something substantial, then please add a note of your changes to whichever of the directory `<path/to/deal.II/fork>/doc/news/changes/[minor/major/incompatibilities]` best suits the nature of the patch.
5. When you're finished committing all of your changes to your local repository, you can push them all upstream to your Github repository.
6. Now you're set to open a pull request, which is typically done from GitHub's website. Once you've done this one or more of the principal developers, or fellow members of the community, may review your changes and engage you on them. We may then go through a (hopefully succinct) stage of iterating your developments until all parties are satisfied with the proposed changes. After this the project, incorporating the proposed modifications, will be built and tested on a remote machine to check that the additions to not break any existing functionality. If these final tests pass then your feature branch will be merged by one of the principal developers.

Using the command line, steps 2-5 would be achieved in the following manner:
[FINISH ME]
```sh
$ cd <path/to/a/working/folder>
$ git clone git@github.com:<username>/dealii.git # Clone forked version of deal.II
$ cd dealii # Work within the local development folder 
$ # Create new feature branch based off of `origin/master`
$ # ...Make some changes...
$ # git add
$ # Indentation script
$ # git commit
$ # git push
```
There's also a tool called [`hub`](https://github.com/github/hub) that facilitates interactions with GitHub (e.g. forking a repository, opening pull requests) from the command line.

For more detailed information, here’s a few links that may be useful:
- [Forking a project](https://help.github.com/articles/fork-a-repo/)
- [Creating branches in GitHub](https://help.github.com/articles/creating-and-deleting-branches-within-your-repository/)
- [Creating a pull request from a fork](https://help.github.com/articles/creating-a-pull-request-from-a-fork/)
- [About pull requests](https://help.github.com/articles/about-pull-requests/)
- [About pull request merges](https://help.github.com/articles/about-pull-request-merges/)
- [Git and workflows](https://www.atlassian.com/git/tutorials/comparing-workflows)