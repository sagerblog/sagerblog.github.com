---
layout: post
title: "git分支的使用"
date: 2013-01-08 10:32
comments: true
categories: ['git', 'vcs']
---
###git checkout -b sager-profile###
创建本地分支

###git checkout sager-profile###
切换分支到sager-profile

<!--more-->
###git checkout master###
切换分支回master

###git push origin sager-profile:sager-profile###
推送本地分支sager-profile到远程

###git branch###
查看本地分支

###git branch -r###
查看远程分支

###git branch -d sager-profile###
删除本地分支

###git push origin :sager-profile###
删除远程分支,语法同推送分支到远程， 只是将一个空分支推送到远程指定分支，也就是删掉远程分支

###git branch -r -d origin/sager-profile###
如果上面的语句执行失败，可以使用这个删除远程分支

