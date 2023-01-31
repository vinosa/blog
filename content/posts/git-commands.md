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

### Rebase

```
git rebase -i master
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
```

