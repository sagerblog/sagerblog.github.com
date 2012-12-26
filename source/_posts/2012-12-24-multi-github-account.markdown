---
layout: post
title: "使用多个ssh key登录github不同账号"
date: 2012-12-24 14:09
comments: true
categories: [github, ssh]
---
使用ssh key的时候可以通过config文件指定不同域名使用不同的key文件，因此通过在hosts中设置不同的本地域名来使用多个ssh key文件登陆不同的github账号。

例如我们有账号a和账号b，对应的ssh key文件分别是id_rsa_a和id_rsa_b，首先修改/etc/hosts文件，设置两个本地域名指向github.com
{% codeblock /etc/hosts lang:sh %}
a.github.com    github.com  #a.github.com可以是任意文字
b.github.com    github.com
{% endcodeblock %}

修改用户的ssh配置文件~/.ssh/config，示例如下：
<!-- more -->
{% codeblock ~/.ssh/config lang:sh%}
Host a.github.com
    User git 
    Hostname github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa_a
Host b.github.com
    User git 
    Hostname github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa_b
{% endcodeblock %}

然后在你的git仓库的远程地址中，将对应的a.github.com和b.github.com替换原来的github.com，参考下面的变化：
{% codeblock git clone lang:sh%}
git clone git@a.githuc.com:sagerblog/sagerblog.github.com
{% endcodeblock %}





