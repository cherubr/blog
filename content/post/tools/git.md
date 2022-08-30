---
title: "Git"
date: 2022-08-29T16:34:52+08:00
categories: ["版本控制工具"]
tags: ["Git Submodule", "Git常见命令","Git 子模块"]
#draft: true
---
# Git
- 什么是git子模块（Submodule）子模块
- merge | rebase

## 1. 什么是git子模块
- 子模块允许您将 Git 存储库保留为另一个 Git 存储库的子目录。 这使您可以将另一个存储库克隆到您的项目中并保持您的提交分开。 
### 1.1 添加子模块
- 在现有仓库基础之上执行命令。
```bash
  git submodule add ${submodule.uri}
```
会发现主目录（对应子模块目录上级或者说根目录）生成一个.gitmodules配置文件，里面有子模块对应的相关信息。
同时，git不会跟踪对应子模块的更改。
### 1.2 git clone 带有子模块项目
- clone 只能得到对应子模块的文件夹，里面没有任何文件
- 通过一下命令得到对应完整文件
```bash
初始化您的本地配置文件  
git submodule init  
从该项目中获取所有数据并检查您的超级项目中列出的相应提交  
git submodule update
要初始化,获取和签出任何嵌套的子模块
git submodule update --init --recursive
```
- --recurse-submodules 克隆项目同时克隆子模块项目
```bash
git clone --recurse-submodules ${git.url}
```
### 1.3 子模块更新
- 如果要检查子模块中的新工作，可以进入目录并运行git fetch和git merge上游分支以更新本地代码。
- git pull = git fetch  + git merge
- 如果您不想在子目录中手动获取和合并。 
```
git submodule update --remote
```
此命令将假定您要将检出更新到远程子模块存储库的默认分支（远程指向的分支HEAD）。 但是，您可以根据需要将其设置为不同的值
```bash
git config -f .gitmodules submodule.{submodule.name}.branch stable
```
### 1.4 子模块问题
- 当我们运行git submodule update命令从子模块存储库中获取更改时，Git 将获取更改并更新子目录中的文件，但会使子存储库处于所谓的“分离 HEAD”状态
```
 1.切换到子模块目录执行 
 git checkout ${branch.name}
 2.切换回到刚才主目录执行
 git submodule update --remote --merge|--rebase ${submodule.name} （${submodule.name} 存在多个子模块带上对应名称,否则可以忽略）
 3.注意：这里是将远程的分支和本地新建的分支进行合并，之后的拉取项目必须带上 --meger 或者 --rebase,否则还会出现分离现象。
 当我们在子模块工作区间进行提交之后，忘记了（--merge , --rebase）
 忘记也没有关系，这里切换到子模块对应目录，你刚才生成的分支依然存在 ，切换到对应分支，返回主目录重新进行meger和rebase操作。
```
## 2. merge和rebase





