# Git

## Config

Show Config list: `git config -l`

Global user: `git config --global user.name`

Local user: `git config user.name`


## Cloning a new repository with Token

```
git clone https://[TOKEN]@github.com/[REPO-OWNER]/[REPO-NAME]
```


## `–force-with-lease`

[https://blog.developer.atlassian.com/force-with-lease/](https://blog.developer.atlassian.com/force-with-lease/)

## Stash

[https://git-scm.com/docs/git-stash](https://git-scm.com/docs/git-stash)

[https://git-scm.com/book/en/v2/Git-Tools-Stashing-and-Cleaning](https://git-scm.com/book/en/v2/Git-Tools-Stashing-and-Cleaning)

[https://www.atlassian.com/git/tutorials/saving-changes/git-stash](https://www.atlassian.com/git/tutorials/saving-changes/git-stash)


### drop & clean
`git stash drop`

`git stash clear`

### New branch from stash
`git stash branch <BRANCH_NAME> <STASH_ID>`

`git stash branch branch_1 stash@{1}`


