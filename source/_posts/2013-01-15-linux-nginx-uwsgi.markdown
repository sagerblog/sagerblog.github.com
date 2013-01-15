---
layout: post
title: "Linux配置Nginx+uWsgi环境"
date: 2013-01-15 12:49
comments: true
categories: [nginx, uwsgi, linux, config, python]
---

转Python后碰到的最大的问题就是服务器配置，产品环境最终还是需要用到Nginx+uWsgi，折腾过好久，把操作记录放在这里，方便查询

我的环境：RHEL6，python2.6.6，Nginx1.2.6，uWsgi1.4.4（都是从官方网站下载的最新版本）


###1.环境准备###

先更新系统，并安装编译环境等等。
```bash Prepare environment
yum update
yum install glib2-devel openssl-devel pcre-devel bzip2-devel gzip-devel\
    python python-devel libxml2 libxml2-devel python-setuptools zlib-devel\
    wget pcre pcre-devel sudo gcc make autoconf automake
```

###2.编译安装Nginx###
先到[Nginx官网](http://nginx.org/en/download.html)下载最新稳定版本的Nginx1.2.6，编译安装
<!--more-->
```bash Compile and install nginx
cd /usr/local/src
wget http://nginx.org/download/nginx-1.2.6.tar.gz
tar -zxvf nginx-1.2.6.tar.gz
cd nginx-1.2.6
./configure \
    --user=nginx \
    --group=nginx \
    --prefix=/usr/share \
    --sbin-path=/usr/sbin/nginx \
    --conf-path=/etc/nginx/nginx.conf \
    --error-log-path=/var/log/nginx/error.log \
    --http-log-path=/var/log/nginx/access.log \
    --pid-path=/var/log/run/nginx.pid \
    --lock-path=/var/log/lock/subsys/nginx \
    --with-http_ssl_module \
    --with-http_realip_module \
    --with-http_addition_module \
    --with-http_sub_module \
    --with-http_dav_module \
    --with-http_flv_module \
    --with-http_gzip_static_module \
    --with-http_stub_status_module \
    --with-mail \
    --with-mail_ssl_module
make
make install
```
更多的编译选项参考：[http://wiki.nginx.org/NginxInstallOptions](http://wiki.nginx.org/NginxInstallOptions)

###3.Nginx环境配置###
####3.1为Nginx建立用户####
```bash add user nginx
useradd -c "Nginx user" -s /bin/false -r -d /var/lib/nginx nginx
adduser --system --no-create-home  nginx
```
####3.2运行Nginx####
```bash run nginx
/usr/sbin/nginx
```
####3.3配置文件####
```bash nginx conf
/etc/nginx/nginx.conf
echo "NGINX_CONF_FILE=/etc/nginx/nginx.conf" > /etc/sysconfig/nginx
```

###4.编译安装uWsgi###
进入uWsgi的[官方网站](http://projects.unbit.it/uwsgi/)，下载它的当前稳定版本，我这里下载的是1.4.4版本。
```bash Compiler and install uWsgi
cd /usr/local/src
wget http://projects.unbit.it/downloads/uwsgi-1.4.4.tar.gz
tar -zxvf uwsgi-1.4.4.tar.gz
mv uwsgi-1.4.4 uwsgi
cd uwsgi
python setup.py build
make
mv uwsgi /usr/bin  #move execuable file to /usr/bin
```

###5.配置uWsgi###
详细配置方式可以参考官网上的配置说明：[http://projects.unbit.it/uwsgi/wiki/Quickstart](http://projects.unbit.it/uwsgi/wiki/Quickstart)
我以项目中的两个模块配置文件示例如下（web模块和api模块，分别走两个不同的端口）
```bash web.ini config for uwsgi
[uwsgi]
autoload = true
master = true
uid=sager
gid=sager
workers = 2
socket = 127.0.0.1:8081
module = web-uwsgi
chdir = /home/sager/project
pythonpath = /opt/pyenv
virtualenv = /opt/pyenv
```
```bash api.ini config for uwsgi
[uwsgi]
autoload = true
uid=sager
gid=sager
master = true
workers =2
socket = 127.0.0.1:8080
module = api-uwsgi
chdir = /home/sager/project
pythonpath = /opt/pyenv
virtualenv = /opt/pyenv
```
###6.启动Nginx和uWsgi服务###
```bash start service
nginx 
uwsgi -i api.ini
uwsgi -i web.ini
```

###7.配置Nginx支持uWsgi###
简化的nginx.conf文件，增加一条include内容，见最后一行
```bash nginx.conf
user  nginx;
worker_processes  1;
events {
    worker_connections  1024;
}
http { 
    include       mime.types; 
    default_type  application/octet-stream; 
    sendfile        on; 
    keepalive_timeout  65; 
    include /etc/nginx/sites-enabled/*; 
} 
```
然后在/etc/nginx/sites-enabled目录中增加单个的server配置内容，参考如下
```bash sager-project.conf
server { 
    listen 80; 
    server_name localhost;  
    location /api/ { 
        rewrite ^/api/(.*)$ /$1 break; 
        include uwsgi_params; 
        uwsgi_pass 127.0.0.1:8080;  
    } 
    location / { 
        include uwsgi_params; 
        uwsgi_pass 127.0.0.1:8081;  
    } 
}
```
重启服务后，通过http://localhost即可访问项目主页了，web的请求会直接转发给8081端口，而http://localhost/api的请求则会转发到8080端口

至此，我们的配置完成了，可以参考下一篇文章，将nginx和uwsgi配置为系统服务，并开机自动启动

