---
layout: post
title:  "Revert commit"
date:   2025-06-24
categories: [Git]
---

## Tìm id của commit (dạng hash)
```
git show --summary
git log
git log [commit id]
```

## Lệnh revert commit 
`git revert -m 1 <merge-commit>`

## Lý thuyết
git merge feature-branch
This creates a merge commit with:

Parent 1: main (the branch you were on)

Parent 2: feature-branch

git show --summary
git log
git revert -m 1  <merge-commit>

git revert -m 1  9ba06b4d72f8d60484e23d