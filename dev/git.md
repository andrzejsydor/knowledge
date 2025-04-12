# Git

## Config

Show Config list: `git config -l`

Global user: `git config --global user.name`

Local user: `git config user.name`

Local user email: `git config --local user.email "name@mail.com`


## Cloning a new repository with Token

```
git clone https://[TOKEN]@github.com/[REPO-OWNER]/[REPO-NAME]
```


## `–force-with-lease`

### `git push --force-with-lease` is a safer variant of `git push --force`. Instead of blindly overwriting the remote branch, it checks if the remote branch has changed since you last pulled. If it detects new commits on the remote, the push will fail instead of overwriting them. This prevents accidentally discarding someone else’s work.

[https://blog.developer.atlassian.com/force-with-lease/](https://blog.developer.atlassian.com/force-with-lease/)

## Change commit author

```
git commit --amend --author="Author Name <new.email@example.com>" --no-edit
```


## Stash

[https://git-scm.com/docs/git-stash](https://git-scm.com/docs/git-stash)

[https://git-scm.com/book/en/v2/Git-Tools-Stashing-and-Cleaning](https://git-scm.com/book/en/v2/Git-Tools-Stashing-and-Cleaning)

[https://www.atlassian.com/git/tutorials/saving-changes/git-stash](https://www.atlassian.com/git/tutorials/saving-changes/git-stash)

## Notes

[https://git-scm.com/docs/git-notes](git-notes) - Add or inspect object notes)

### drop & clean
`git stash drop`

`git stash clear`

### New branch from stash
`git stash branch <BRANCH_NAME> <STASH_ID>`

`git stash branch branch_1 stash@{1}`


