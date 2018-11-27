---
title: git操作命令
date: 2018-11-26 11:42:28
tags: 
---
<!-- more -->
## ssh公钥
查看本机是否有ssh公钥，通常一个主机对应一个密钥
```bash
$ cd ~/.ssh
$ ls
```
通常.pub后缀的文件就是公钥

创建ssh公钥
```bash
$ ssh-keygen
```

查看本机ssh公钥
```
$ cat ~/.ssh/id_rsa.pub
```

## git status查看工作区状态
```
# git status
- 存在未跟踪文件
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

        README

nothing added to commit but untracked files present (use "git add" to track)

- 将未跟踪文件加入跟踪
$ git add README
再次执行$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   README
此时该文件为暂存状态

- 存在已修改文件
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   index.html
说明已跟踪文件内容发生变化，但还未放入暂存区

- 将已修改文件加入暂存区
$ git add index.htm
再次执行$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   README
        modified:   index.html
这时存在一个新增文件和一个改动过的文件，它们都已暂存，下次commit会一并提交到仓库

- 工作目录干净，不存在未跟踪或修改过的文件
On branch master
Your branch is up to date with 'origin/master'.
nothing to commit, working tree clean
```

## 常见操作
```
# 从已跟踪文件清单中移除（即暂缓区），然后提交
$ git rm

# 移动文件（前一个文件是要移动的文件，后一个是移动后的文件）
$ git mv file_from file_to
这一部相当于运行了下面三步：
$ mv README.txt README
$ git rm README.txt
$ git add README

# 查看历史提交记录
$ git log -l 3  // l指list，后跟想要查看的历史记录条目数
```

## 撤销上一次的commit
```
# 查看提交记录，找到上次提交的 commit id
$ git log

# 撤销，使代码恢复到前一次 commit id 对应的版本
$ git reset --hard 748f409386c9fb1f4a51f29a45bad6bff24bf284

# 撤销，但不对代码修改进行撤销，仍可以通过 git commit 重新提交对本地代码的修改
$ git reset 748f409386c9fb1f4a51f29a45bad6bff24bf284
```
![image](https://wx4.sinaimg.cn/large/9f1d2bbagy1fxmrcy11imj21t01e8all.jpg)
 
