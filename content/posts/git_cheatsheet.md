---
title: 'Git - Cheat Sheet'
date: 2021-03-14T15:00:11+06:00
draft: false
featuredImg: ""
tags: 
  - git
---

Git on the commandline

- [Create](#create)
- [Browse](#browse)
- [Revert](#revert)
- [Update](#update)
- [Branch](#branch)
- [Publish](#publish)
- [Conflicts](#conflitcs)

## Quickstart

### Setup

Configuring user information used across all local repositories
```
$ git config --global user.name "Alejandro Ponce"
$ git config --global user.email "aleo.poncelet@gmail.com"
$ git config --global color.ui auto
```

### SSH key

Connecting to GitHub with SSH 
```
$ ssh-keygen -t ed25519 -C "aleo.poncelet@gmail.com"
$ eval "$(ssh-agent -s)"
> Agent pid 59566
$ ssh-add ~/.ssh/id_ed25519
$ cat ~/.ssh/id_ed25519.pub
```
### Basic commands
```
$ git init
$ git add .
$ git commit -m "first commit"
$ git branch -M main
$ git remote add origin git@github.com:aleoponcelet/<repo>.git
$ git push -u origin main
```
---
## Create

From existing data
```
$ git init "project name"
```
From existing repository
```
$ git clone <url>
```

## Browse

Files changed in working directory
```
$ git status
```
Changes made to tracked files
```
$ git diff
```
What changed between id1 and id2
```
$ git diff <id1> <id2>
```
History of changes
```
$ git log
```
History of changes for file with diffs
```
$ git log -p <file> <directory>
```
Who changed what and whend in a file
```
$ git blame <file>
```
A commit identified by id
```
$ git show <id>
```
A specific file from a specific id
```
$ git show <id>:<file>
```
All local branches
```
$ git branch
```

## Revert

Return to the last commited state
```
$ git reset --hard
```
Revert the last commit
```
$ git revert HEAD
```
Revert specific commit
```
$ git revert <id>
```
Fix the last commit
```
$ git commit -a --amend
```
Checkout the id version of a file
```
$ git checkout <id> <file>
```

## Update

Fetch latest changes from origin
```
$ git fetch
```
Pull latest changes from origin
```
$ git pull
```

## Branch

Switch to a branch
```
$ git checkout <branch>
```
Merge branch1 into branch2
```
$ git checkout <branch2>
$ git merge <branch1>
```
Create a branch based on head
```
$ git branch <branch name>
```
Create a branch based on other and switch to it
```
$ git checkout -b <branch> <other>
```
Delete a branch
```
$ git branch -d <branch>
```

## Publish

Commit all your local changes
```
$ git commit -a
```
Prepare a patch for other developers
```
$ git format-patch origin
```
Push changes to origin
```
$ git push
```
Make a version or milestone
```
$ git tag v1.0
```

## Conflicts

View merge conflicts
```
$ git diff
```
View merge conflicts against base file
```
$ git diff --base <file>
```
View merge conflicts against your changes
```
$ git diff --ours <file>
```
View merge conflicts against others changes
```
$ git diff --theirs <file>
```
Discard a conflicting patch
```
$ git reset --hard
$ git rebase --skip
```
After resolving conflicts, merge with
```
$ git add <conflicting_file>
$ git rebase --continue
```