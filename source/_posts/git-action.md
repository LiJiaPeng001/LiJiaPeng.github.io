---
title: git常用操作
date: 2019-11-01 10:49:27
tags: git
---

# 分支操作

```
git checkout -b dev
```

git checkout 加上 -b 参数表示创建并切换，相对于以下两条命令

```
git branch dev
git checkout dev
```

查看当前分支 -a 可以查看远程分支

```
git branch -a
```

合并分支

```
git merge dev
```

删除本地分支

```
git branch -d dev
```

切换分支完提交到远程 -d 删除远程仓库分支

```
git push origin dev
git push -d origin dev
```

# 标签操作

先切换到需要打标签的分支上，然后敲命令 git tag <name> 就可以打一个新标签

```
git checkout dev
git tag v1.0.0
```

如果要打在之前的 commit，可以这样写

```
git log --pretty=oneline --abbrev-commit
git tag v0.9 f52c633
```

查看标签列表,git show 可以查看标签信息

```
git tag
git show <tag>
```

标签提交到远程

```
git push origin <分支名> --tags
```
