# Contributing code via Git and GitHub

deal.II is developed using [Git](https://git-scm.com) as a version control tool, [GitHub](https://github.com/about) as the central source-code host and a [branch-based workflow](https://guides.github.com/introduction/flow/). This article supplements the information given in the [CONTRIBUTING](https://github.com/dealii/dealii/blob/master/CONTRIBUTING.md) documentation and this [detailed video lecture](http://www.math.colostate.edu/%7Ebangerth/videos.676.32.8.html).

## The basic steps

To contribute a patch back to the project (which we greatly appreciate), the following summarises the steps that you need to take to do this:

1. Create a GitHub account and _fork_ the [deal.II project](https://github.com/dealii/dealii).
2. Make a local _clone_ of your fork. This will provide you a local copy of your fork of deal.II, and will configure the local Git project such that the you can _push_ your changes _upstream_ to your _remote_ version of deal.II as hosted on GitHub. The name traditionally given to this remote repository is `origin`.
3. Create a new feature branch on which you will be making changes. This marks the point from which your copy of the project differs from that of the main development branch of the parent repository (that's ours). Since we do our development in a branch called `master`, you'll want to use this `master` branch as the source of your branch. Better yet, in some IDE's if you choose `origin/master` as the branch source, then not only will Git use `master` as the version on which to _base_ your new feature branch, but will also configure the branch such that the changes that you make can be _pushed_ upstream to your fork of the repository. If you're doing things on the command line then you might have to perform this configuration later.
4. Now is the right time to make your changes and additions. Once your made changes to the project (by modifying existing files or creating new ones) you _commit_ each change. With this commit, Git records the set of differences between your updated view of the project and the previous one. With each commit you can write a short message describing what your particular set of changes does. We typically write a very short summary in the first line of the commit message. If necessary this is followed by a longer description (in an IDE, this would be from the third line onwards). Within the larger description you can also use [specific keywords](https://help.github.com/articles/closing-issues-using-keywords/) that, for example, allows cross-referencing of issues or pull-requests within GitHub or trigger the closing of issues when a pull-request is merged.
    - We have somewhat strict guidelines as to the formatting of all submitted code, and we test that the patches that are submitted conform to these guidelines. In order to satisfy the tester that automatically checks the code formatting, we recommend that you first run the script found at `<path/to/deal.II/fork>/contrib/utilities/indent` before committing your changes. See https://github.com/dealii/dealii/wiki/Indentation for more details.
    - If you submit a patch that adds a new feature or something substantial, then please add a note of your changes to whichever of the directory `<path/to/deal.II/fork>/doc/news/changes/[minor/major/incompatibilities]` best suits the nature of the patch.
5. When you're finished committing all of your changes to your local repository, you can push them all upstream to your Github repository.
6. Now you're set to open a pull request, which is typically done from GitHub's website. Once you've done this one or more of the principal developers, or fellow members of the community, may review your changes and engage you on them. We may then go through a (hopefully succinct) stage of iterating your developments until all parties are satisfied with the proposed changes. After this the project, incorporating the proposed modifications, will be built and tested on a remote machine to check that the additions to not break any existing functionality. If these final tests pass then your feature branch will be merged by one of the principal developers.

## An example

Using the command line, steps 2-5 would be achieved in the following manner:

2. Clone the forked repository
```sh
$ cd <path/to/a/working/folder>
$ git clone git@github.com:<username>/dealii.git # Clone forked version of deal.II
$ cd dealii # Work within the local development folder 
$ git branch -vv # Show local branches and their configuration
* master 0e5d7e8 [origin/master] update VERSION for release 
```
Here we can see that there is one local branch called `master` that is configured to synchronize (i.e. _push_ and _pull_) with a branch called `master` on the remote repository called `origin`. The last commit hash on the local `master` is `0e5d7e8` and has the commit message `update VERSION for release`.

3. Switch to a new feature branch and configure it to push changes to your repository on GitHub.
First we create a new branch called `my_amazing_new_feature`
```sh
$ git checkout -b my_amazing_new_feature # Checkout a new branch with the given name
Switched to a new branch 'my_amazing_new_feature'
$ git branch -vv # Show detailed information about the local branches in this repository
  master                 0e5d7e8 [origin/master] update VERSION for release
* my_amazing_new_feature 0e5d7e8 update VERSION for release
```
which we see is, essentially, a duplicate of `master` at this point. However, it is not configured to track any remote branch. This is because we were originally on the local `master` branch before switching to our new one.

4. Now you can make some changes. Here we edit a file and add a new one
```sh
$ vi README.md
# ... edit this file...
$ touch MORE_DOCUMENTATION.md # Create a new file
```
You can query Git as to which files have been altered since the last commit
```sh
$ git status # Display the current status of the repository
On branch my_amazing_new_feature
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   README.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	MORE_DOCUMENTATION.md

no changes added to commit (use "git add" and/or "git commit -a")
```
and you can see the exact changes made to a file
```sh
$ git diff README.md # Print the changes made to the given file
diff --git a/README.md b/README.md
index 733e289308..1564ac6e5c 100644
--- a/README.md
+++ b/README.md
@@ -45,7 +45,7 @@ on how to set this up.
 License:
 --------

-Please see the file [./LICENSE](LICENSE) for details
+Please see the file [./LICENSE](LICENSE) for details.

 Further information:
 --------------------
```
Here we fixed a typographical error - a small but valuable contribution!
At this point it appears that we're ready to inform Git of the changes that we've made, but remember the styling that deal.II expects? We should first ensure that our changes adheres to them.
```sh
$ ./contrib/utilities/indent # Called from the base directory.
```
The next step is to add these changes to Git's index (_staging_), which is reflected in a change in status of these two files.
```sh
$ git add README.md MORE_DOCUMENTATION.md # Add to the index the current state of these files
$ git status
On branch my_amazing_new_feature
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   MORE_DOCUMENTATION.md
	modified:   README.md
```
Now we can save the changes to these two files by committing them.
```sh
$ git commit -m "A short description of my changes" -m "A more lengthy description of the changes that I've made. Here I can go into depth explaining what the purpose of the changes are. Fixes #123. Closed #456." # Create a new commit with both a short and a long description.
$ git branch -vv
  master                 0e5d7e8 [origin/master] update VERSION for release/vector_adaptor
* my_amazing_new_feature a4f12d6 A short description of my changes
```
You can see that your branch differs from the `master` branch. We can also inspect the change more carefully via
```sh
$ git log -n 1 # Show the last entry in the history
commit a4f12d6ee26f3fefab24c758807261cd91f2a46e
Author: <Author's name> <Author's email address>
Date:   Sat Nov 11 16:25:26 2017 +0100

    A short description of my changes

    A more lengthy description of the changes that I've made. Here I can go into depth explaining what the purpose of the changes are. Fixes #123. Closed #456.
```
In this example commit, we've used two keywords that show that upon merging issue number 123 is resolved and some discussion conducted in issue number 456 is no longer relevant.

5. Lastly, you can get Git to push your changes to your remote version of the deal.II repository.
```sh
$ git push -u origin my_amazing_new_feature # Push a specified branch to the specified remote destination
```
In particular, this tells Git to synchronize (or create) the branch `my_amazing_new_feature` on the remote repository labelled `origin`. We could also verify exactly where `origin` points before pushing our changes:
```sh
$ git remote -vv
origin	git@github.com:<username>/dealii.git (fetch)
origin	git@github.com:<username>/dealii.git (push)
```

## Further information

There's also a tool called [`hub`](https://github.com/github/hub) that facilitates interactions with GitHub (e.g. forking a repository, opening pull requests) from the command line.

For more detailed information, here’s a few links that may be useful:
- [Forking a project](https://help.github.com/articles/fork-a-repo/)
- [Creating branches in GitHub](https://help.github.com/articles/creating-and-deleting-branches-within-your-repository/)
- [Creating a pull request from a fork](https://help.github.com/articles/creating-a-pull-request-from-a-fork/)
- [About pull requests](https://help.github.com/articles/about-pull-requests/)
- [About pull request merges](https://help.github.com/articles/about-pull-request-merges/)
- [Git and workflows](https://www.atlassian.com/git/tutorials/comparing-workflows)