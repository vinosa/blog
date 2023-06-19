---
title: "Git Tips"
date: 2023-06-13T14:43:15+02:00
draft: false
categories:
- Development
- Git
tags:
- dev
- git
---
### Move last 2 commits from master to develop

```
git checkout master
git branch -d develop
git log
git reset --hard HEAD~2
git checkout <sha1_of_last_orphan_commit>
git checkout -b develop 
```

### Correct master branched to develop

```
git checkout develop
git log
git reset --hard HEAD~1
git checkout master
git reset --hard <sha1_orphan_merge_commit>
git comit --amend // rename commit
```

### Search bugged commit

```
git bisect start

// define borders
git bisect good [ref] // bug is not present
git bisect bad [ref]  // bug is present

git bisect good/bad // if bug is present
...
or
git bisect run ./test.sh (script returns 0 for good, or anything else for bad)

git bisect reset
```

### Init server repository

```
git init --bare
```

## Hooks

### Local

* pre-commit
* prepare-commit-msg
* commit-msg
* post-commit
* post-checkout
* pre-rebase

### Server

* pre-receive // on push
* update
* post-receive // notification, ci etc
