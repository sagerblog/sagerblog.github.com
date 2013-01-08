---
layout: post
title: "Ubuntu安装Dropbox后无法连接服务器的解决方法"
date: 2012-12-26 07:50
comments: true
categories: [linux, dropbox, it] 
---

由于工作需要，操作系统从Mac换到了Ubuntu，现在这个年代，谁没个云存储啥的，Dropbox当然是首选（通过某些方式已经将免费空间提升到25G了），装完Ubuntu后第一个事情就是装Dropbox。 

于是我就马不停蹄地下载了Dropbox的安装包然后装到Ubuntu上，装完一打开，弹出这么个提示：“Trouble connecting to Dropbox server.Maybe your internet connection is down,or you need to set your http_proxy environment variable”，主要意思就说无法连接上dropbox的服务器。

然后我把hosts改了，问题依旧…，只有祭出google大神了
<!--more-->
原来装了那个deb包还不算装完，第一次启动时还要下载一个.tar.gz的包，明显这个包的地址在墙外了。

所以需要到这个地址 <http://www.getdropbox.com/download?plat=lnx.x86> 下载这个.tar.gz包，当然要先爬墙。（我把这个文件传到了华为网盘 <http://dl.vmall.com/c0sgq6m4m6> ，无法爬墙的同学如果信得过我的话可以下载）

然后在下载到的dropbox-lnx.x86-1.2.52.tar.gz文件解压到主文件夹，是个隐藏文件夹.dropbox-dist, 按“Ctrl + h”显示隐藏文件

最后打开dropbox即可正常使用啦～

