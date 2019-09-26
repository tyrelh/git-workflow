# Git Workflow
## A simple workflow for teams
A branching workflow keeps things sane when multiple devs are working on the same codebase.
* A branch should represent the smallest atomic addition of functionality (ideally).
* In order to stay up to date, rebase your branch onto `origin/master` often.
* In order to keep others up to date, and reduce merge conflicts on your behalf, don't merge your branch as soon as possible, adhere to the first point (within reason).
* We will use a rebasing workflow to keep our master branch linear.

### Starting a new unit of work (branch):
```bash
git checkout master
git fetch
```
Make sure you are now on master, and do not habe any work on`master, because the next step will nuke it. Use `git stash` before-hand if you do.
```bash
git reset --hard origin/master
git checkout -b 'branch-name-goes-here'
```

### Checking the staging area:
Git performs action on files that have been placed in the staging area, which acts as a triage. To see what files are in the staging area:
```bash
git status
```

### To add a file/directory to the staging area:
```bash
# To add a single file
git add some_file.py

# To add every file in the current directory
git add .
```
### To `commit`, i.e. save files in the staging area to the local git repo:
```bash
git commit -m "A <50 character title describing WHAT functionality the commit adds"
```
[Commit Message Best Practices](https://chris.beams.io/posts/git-commit/). Try not to commit junk files. If unsure, ask your manager.

### To push your branch to the remote repo (GitHub):
```bash
git push origin branch-name
```

### To merge your work into master:
We use a `no-ff` merge because we have rebased onto master (so that our master branch stays linear), and we want to generate a merge commit for clarity/posterity. The message has the form "Merge branch 'branch-name'" so that it is easy to spot merge commits in the log.
```bash
git checkout your-branch-to-merge
git fetch
git rebase origin/master
```
If you have merge conflicts at this stage, you will need to fix them. After, repeat from the beginning.
```bash
git checkout master
```
Make sure you are now on master and do not have any work on master, because the next step will nuke it.
```bash
git reset --hard origin/master
git merge --no-ff 'your-branch-to-merge' -m "Merge branch 'your-branch-to-merge'"
git push origin master
```

### NEVER DO THIS:
Never edit public history. You can always edit/change local and remote history in your own branches (using commit amending, interactive rebasing, resetting, force pushing, etc.). However, you should NEVER change history on `remote/origin/master`, and you should never edit other people's branches (unless you _really_ know what you are doing and have the author's permission). 

### Other useful git commands:
See log of git commits:
```bash
git log
```
See branches:
```bash
git branch
```
Selectively add to staging area:
```bash
git diff
git diff .
git diff your-file.py
```
Interactive rebase for changing history (only change your own history):
```bash
git rebase -i <commit hash>
```
To add changes to your last commit:
```bash
git add changed_file.py
git commit --ammend
```
