Git Commands


_A list of my commonly used Git commands_

### Getting  Projects

| Command | Description |
| ------- | ----------- |
| `git clone https://github.com/Mosweed/yaduhsfuf.git` | Create a local copy of a remote repository |

### Basic Snapshotting

| Command | Description |
| ------- | ----------- |
| `git status` | Check status |
| `git add -A` | Add all new and changed files to the staging area |
| `git commit -m "[commit message]"` | Commit changes |

### Branching & Merging

| Command | Description |
| ------- | ----------- |
| `git branch` | List branches (the asterisk denotes the current branch) |
| `git branch -a` | List all branches (local and remote) |
| `git branch [branch name]` | Create a new branch |
| `git branch -d [branch name]` | Delete a branch |
| `git push origin --delete [branch name]` | Delete a remote branch |
| `git checkout -b [branch name]` | Create a new branch and switch to it |
| `git checkout [branch name]` | Switch to a branch |
| `git merge [branch name]` | Merge a branch into the active branch |
| `git merge [source branch] [target branch]` | Merge a branch into a target branch |

### Sharing & Updating Projects

| Command | Description |
| ------- | ----------- |
| `git push` | Push changes to remote repository (remembered branch) |
| `git pull` | Update local repository to the newest commit |
