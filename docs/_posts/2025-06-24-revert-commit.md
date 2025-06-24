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

## Ví dụ

commit abcdecgf1234 (tag: v2.0.1, origin/backup/2024, backup/2024)

Author: email

Date:   Mon Mar 24 15:36:51 2025 +0700

    Thêm cột giảng viên ký bộ môn ký

git checkout -b feature/new-feature a1b2c3d4

git checkout -b  backup/2024 abcdecgf1234

git tag v2.0.1 abcdecgf1234

============================

commit abcdecgf1234 (tag: v2.0.1, origin/backup/2024, backup/2024)

Author:email

Date:   Mon Mar 24 15:36:51 2025 +0700


    Thêm cột giảng viên ký bộ môn ký
 

Cập nhật theo công thức tính mới

```
git revert <old-commit>..HEAD
git revert a1b2c3d4..HEAD
git revert abcdecgf1234..HEAD
```

```
git reset --hard <commit-hash>
git push origin HEAD --force
```

```
git reset --hard abcdecgf1234
git push origin HEAD --force
```