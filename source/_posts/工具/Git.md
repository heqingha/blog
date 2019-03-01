---
title: Git
categories:
  - web skill
tags:
  - Git
date: 2017-10-24 20:06:50
---
> 零基础浅显易懂的Git教程,一步步从创建本地到远程仓库, git指令清晰明了

####  安装

* [官网下载](https://git-scm.com/downloads)
<!--more-->
#### 创建版本库

```
  mkdir learngit
  cd learngit

```

#### 将目录变成Git可以管理的仓库

```
git init
ls -ah                //查看.git文件, 看仓库是否建成功

```

#### 新建分支

```
git checkout -b dev   //创建dev分支兵切换到dev 等价于

git branch dev        //创建dev分支
git checkout dev      //切换到dev分支

git branch            //查看当前分支
git branch -d dev     //删除dev分支
git merge master      //合并分支到当前分支, 在当前分支
```
#### 提交文件到暂存区

```
git add readme.txt    //提交指定文件
git add .             //提交所有文件
git status            //查看发生了哪些变化

```

#### 暂存区内容提交到当前分支

```
git commit -m   '提交的提示语'
git status           //  查看发生了哪些变化

```

#### 创建远程仓库

```
ssh-keygen -t rsa -C "youremail@example.com"  //获取秘钥

用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人

登陆GitHub，打开“Account settings”，“SSH Keys”页面：

点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容

```

#### 本地与远程库关联

```
git remote add origin git@github.com:myname/learngit.git

```

#### 将本地仓库推送到远程

```
git push -u origin master    
//远程仓库默叫origin, 第一个推送加-u, Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的//master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令
```

#### 从远程仓库克隆

```
git clone '远程仓库地址'

```

#### 推荐可视化工具(TortoiseGit、SourceTree、GitGUI等等)



