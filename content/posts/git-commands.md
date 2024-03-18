---
title: "Git Commands"
description: "Some Git commands"
date: 2023-01-31T09:28:24+01:00
draft: false
categories:
- Development
- Git
tags:
- dev
- git
---
---

## Branches

### Rename  Local Branch

```
git checkout <old_name>
git branch -m <new_name>
```
### Delete local branch
```
git branch -d branch_name
```
### Delete Remote Branch

```
git push origin --delete my-branch
```

## Files

### New/modified files
```
git status
```

### Track file(s)

```
git add my-file
git add .
```

### Unstage file

```
git reset HEAD myfile
```

## Rewrite History

### Interactive Rebase

```
git rebase -i master
git rebase -i <sha1>
git rebase -i HEAD~3
git rebase --continue
```

### Edit past commit
```
git rebase -i <sha1>

edit <sha1>

git commit --amend

git rebase --continue
```

## Synchronize work

### Pull after force-push (Reset HARD)

```
git reset origin/my-branch --hard
```

## Commits

### Add to last commit

```
git commit --amend
```

### Get first 8 digits of commit

```
git rev-parse HEAD | cut -c 1-8
```

### Cancel commit (creates opposite commit)

```
git revert <sha1> 
```

### Change git user

```
git commit --amend --date=now --author="John Doe <john.doe@yes.com>" --no-edit
```

### Cancel last commit/commit amend (points branch  to the previous commit)

```
git reset --soft HEAD@{1}
```

### Revert file to specific commit

```
git checkout commmit_hash --  path/to/file
```

## Repository

### Set Remote to SSH

```
git remote set-url origin git@github.com:USERNAME/REPOSITORY.git
```

### Clone repository through SSH

```
git clone ssh://git@github.com/Vendor/project.git
```

## Logging

### Show last commit changes

```
git show
```

### Log

```
git log --oneline --all --graph
gitk --all // include orphans
```

### HEAD reference logging
```
git reflog
```

### History details (N last commits)
```
git log -p -<N>
```
### Code evolution
```
git diff <sha1 / ref>
git diff <sha1> <sha1>
git diff HEAD^
git diff HEAD~<N>
```

### Who did it
```
git blame -L [start],[stop] [file]
git blame -L [start],+<N> [file]
```

## Rebase/Reset

### Cancel rebase/reset

```
git reset --hard ORIG_HEAD
```

### Moving branch reference/HEAD back / revert wrong merge

```
git reset --hard HEAD^ // ex. after wrong merge
git reset --hard HEAD~2
git reset --hard sha1 // after wrong reset
git reset --hard tag
```

## Configuration

### Change hooks path

```
git config core.hooksPath <hooks_path>
```

### Init hooks

```
git init --template=<template_dir> // template_dir must have hooks folder

git clone --template=<template_dir>
```

## Submodules

### Add
```
git submodule add <url> <local-path>
```

### Clone
```
git clone <depot>
git submodule init
git submodule update
```
or
```
git clone --recurse-submodules <depot>
``` 

### Check
```
git diff --caches --submodule
```

### Pull rebase policy
```
git submodule update --remote --rebase
```
### Push super repository with submodules
```
git push --recurse-submodules=check
```



