---
layout: post
title: "最小化安装Fedora"
date: 2012-12-24 09:17
comments: true
categories: [linux, it]
---

1.光盘引导

进入到Fedora 16的安装引导界面，按TAB键，在后面追加数字3，Enter，

然后系统引导，弹出一个登录，使用root直接登录即可。

2.运行如下命令

/usr/sbin/anaconda --liveinst --method=livecd:///dev/mapper/live-osimg-min

然后提示完成安装即可。

3.最后需要修改一个默认启动到文本界面
<!-- more -->
cd /etc/systemd/system

ln -s /lib/systemd/system/multi-user.target default.target

4.参考

http://www.linuxreaders.com/2010/09/08/install-fedora-from-live-cd-cli/#.T1YF74da6G4
