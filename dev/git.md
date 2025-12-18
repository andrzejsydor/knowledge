# Git

## Config

| Command | Description |
|---------|-------------|
| `git config -l` | Show config list |
| `git config --global user.name` | Global user |
| `git config user.name` | Local user |

## Cloning with Token

```bash
git clone https://[TOKEN]@github.com/[REPO-OWNER]/[REPO-NAME]
```

## Force with Lease

- [Atlassian: Force with Lease](https://blog.developer.atlassian.com/force-with-lease/)

## Stash

### Resources

- [Git Docs: git-stash](https://git-scm.com/docs/git-stash)
- [Git Book: Stashing and Cleaning](https://git-scm.com/book/en/v2/Git-Tools-Stashing-and-Cleaning)
- [Atlassian: git stash](https://www.atlassian.com/git/tutorials/saving-changes/git-stash)

### Commands

| Command | Description |
|---------|-------------|
| `git stash drop` | Drop a stash |
| `git stash clear` | Clear all stashes |
| `git stash branch <BRANCH_NAME> <STASH_ID>` | Create branch from stash |

**Example:**

```bash
git stash branch branch_1 stash@{1}
```

