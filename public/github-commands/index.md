# Git

This is an overview of some useful Git commands.
<!--more-->

## Set up Git

<strong>Username</strong>
```
git config --global user.name username
```
<br>

<strong>Email</strong>
```
git config --global user.email email
```
<br>

<strong>Vim Editor</strong>
```
git config --global core.editor vim
```
<br>

<strong>Notepad Editor</strong>
```
git config --global core.editor notepad
```
<br>

<strong>Atom Editor</strong>
```
git config --global core.editor "atom --wait"
```
<br>

<strong>Activate rename detection</strong>
```
git config diff.renames true
```
<br>

## Repository
<strong>Create a repository</strong>
```
git init
```
<br>

<strong>Write data into a Object Database</strong>
```
git hash-object -w hello.txt
```
<br>

<strong>Get content of object</strong>
```
git cat-file -p 56d90gfdz
```
<br>

<strong>Get moved files</strong>
```
git log --summary -M90% | grep -e "^ rename"
```
<br>

<strong>Get copied files</strong>
```
git log --summary -C90% | grep -e "^ copy"
```
<br>

<strong>Get history of code lines</strong>
```
git blame -M -C -C -C foo.txt
```
<br>

## Commit

<strong>Choose specific files to commit</strong>
```
git add foo.txt bar.txt
```
<br>

<strong>Choose a directory and everything under it to commit</strong>
```
git add directory/
```
<br>

<strong>Choose the current directory and everything under it to commit</strong>
```
git add .
```
<br>

<strong>Commit</strong>
```
git commit --message "Commit Message"
```
<br>

<strong>Commit all changed files</strong>
```
git commit --all
```
<br>

## Status

```
git status
```
<br>

## Diff

<strong>Workspace vs. Stage</strong>
```
git diff
```
<br>

<strong>Diff for file</strong>
```
git diff foo.txt
```
<br>

<strong>Stage vs. Repository</strong>
```
git diff --staged
```
<br>

<strong>Difference between two files</strong>
```
git diff 1c96fgt main
```
<br>

<strong>Change to the predecessor</strong>
```
git diff p03sr4f^!
```
<br>

<strong>Count of changes</strong>
```
git diff --stat 1c96fgt 72ser18d
```
<br>

<strong>Continuous text</strong>
```
--word-diff
```
<br>

## Reset

<strong>Reset everything</strong>
```
git reset HEAD .
```
<br>

<strong>Reset specific files/directories</strong>
```
git reset HEAD foo.txt src/test/
```
<br>

## Stashing
<strong>Stash changed files</strong>
```
git stash --include-untracked
```
<br>

<strong>Retrieve last saved changes</strong>
```
git stash pop
```
<br>

<strong>Get older stashed changes</strong>
```
git stash pop stash@{1}
```
<br>

<strong>Stash list</strong>
```
git stash list
```
<br>

## Investigate Commits

<strong>Show overview</strong>
```
git show 32rftsgz
```
<br>

<strong>Show file content</strong>
```
git show 32rftsgz:src/main.kt
```
<br>

<strong>Show root directory</strong>
```
git show 32rftsgz:
```
<br>

<strong>Show directory "src"</strong>
```
git show 32rftsgz:src
```
<br>

<strong>Show directory "src" with subdirectories</strong>
```
git ls-tree -r 32rftsgz -- src
```
<br>

## History

<strong>Log output</strong>
```
git log
```
<br>

<strong>Last 3 commits</strong>
```
git log -n 3
```
<br>

<strong>One line per commit</strong>
```
git log --oneline
```
<br>

<strong>Statistics</strong>
```
git log --stat
```
<br>

<strong>Short statistic</strong>
```
git log --shortstat --oneline
```
<br>

<strong>Graph</strong>
```
git log --graph --oneline
```
<br>

## Branches

<strong>List of branches</strong>
```
git branch
```
<br>

<strong>Create branch from current commit</strong>
```
git branch new-branch
```
<br>

<strong>Create branch from any commit</strong>
```
git branch new-branch 672dgzuj
```
<br>

<strong>Create branch from existing branch</strong>
```
git branch new-branch existing-branch
```
<br>

<strong>Switch to a branch</strong>
```
git checkout main
```
<br>

<strong>Create and switch branch</strong>
```
git checkout -b new-branch
```
<br>

<strong>Reset to older commit</strong>
```
git reset --hard rs29bnz1
```
<br>

<strong>Delete not active branch</strong>
```
git branch -d branch-to-delete
```
<br>

<strong>Restore branch when commit hash is known</strong>
```
git branch deleted-branch ek183lp8 
```
<br>

<strong>Get commit hashes</strong>
```
git reflog
```
<br>

## Merge

<strong>Merge feature branch into current branch</strong>
```
git merge feature
```
<br>

<strong>Start mergetool</strong>
```
git mergetool
```
<br>

<strong>Abort merge</strong>
```
git merge --abort
```
<br>

## Working With Repositories

<strong>List of remote tracking branches</strong>
```
git branch --list --remote --verbose
```
<br>

<strong>Fetch: Get branches from other repository</strong>
```
git fetch origin alpha master
```
<br>

<strong>Check integrity of repositories</strong>
```
git fsck
```
<br>

