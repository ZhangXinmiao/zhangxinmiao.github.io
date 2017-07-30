---
title: commit 丢失情况下的自救措施
date: 2017-07-30 17:57:43
tags: 开发工具
---

误操作丢失了 commit 怎么办？

- 找到丢失的commit
```
git reflog
```

- 通过丢失的commit建立新的分支
```
git checkout -b recovery commit编号
```
