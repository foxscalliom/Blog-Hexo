---
abbrlink: acc3d3b0
title: git常用命令总结
date: 2022-01-08
categories: 
   - Git
tags: 
   - github  
---
## 新建仓库
```
echo "# test" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:foxscalliom/test.git
git push -u origin main
```
<!--more-->
## 拉取仓库
```
git remote add origin git@github.com:foxscalliom/test.git
git branch -M main
git push -u origin main
```
## 不保留本地
```
git reset --hard 
git pull origin main
```