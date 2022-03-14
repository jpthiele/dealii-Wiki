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