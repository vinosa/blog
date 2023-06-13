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

