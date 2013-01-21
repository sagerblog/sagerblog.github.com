---
layout: post
title: "配置Nginx和uWsig服务开机自动启动"
date: 2013-01-15 14:53
comments: true
categories: [nginx, uwsgi, linux, service, auto startup]
---

###1.配置Nginx服务开机自动启动###
####1.1创建 Nginx 开机启动脚本####
```bash add auto run
vi /etc/init.d/nginx
```
将以下内容写到该脚本中
<!--more-->
```bash auto start scripts 
#!/bin/bash
nginx=/usr/sbin/nginx
conf=/etc/nginx/nginx.conf

case $1 in
    start)
        echo -n "Starting Nginx"
        $nginx -c $conf
        echo " done"
    ;;

    stop)
        echo -n "Stopping Nginx"
        $nginx -s stop
        echo " done"
    ;;

    test)
        $nginx -t -c $conf
    ;;

    reload)
        echo -n "Reloading Nginx"
        $nginx -s reload
        echo " done"
    ;;

    restart)
        $0 stop
        $0 start
    ;;

    show)
        ps -aux|grep nginx
    ;;

    *)
        echo -n "Usage: $0 {start|restart|reload|stop|test|show}"
    ;;

esac
```

####1.2为 nginx.sh 脚本设置可执行属性####
```bash executable
chmod +x /etc/init.d/nginx
```
####1.3添加 Nginx 为系统服务（开机自动启动）####
```bash add to sysconfig
chkconfig --add nginx
chkconfig nginx on
```
####1.4启动 Nginx####
```bash start service
service nginx start
```
####1.5在不停止 Nginx 服务的情况下平滑变更 Nginx 配置####
修改 /usr/local/webserver/nginx/conf/nginx.conf 配置文件后，请执行以下命令检查配置文件是否正确：
```bash test config and reload
service nginx test
service nginx reload
```

###2.配置uWsgi服务开机自动启动###
####2.1创建 uWsgi 开机启动脚本####
```bash add auto run
vi /etc/init.d/uwsgi
```
将以下内容写到该脚本中
```bash auto start scripts 
#!/bin/bash
uwsgi=/usr/bin/uwsgi
api_conf=/etc/uwsgi/apps-enabled/project-api.ini
web_conf=/etc/uwsgi/apps-enabled/project-web.ini

case $1 in
    start)
        echo -n "Starting uWsgi"
        nohup $uwsgi -i $api_conf >/var/log/uwsgi/project-api.log 2>&1 &
        nohup $uwsgi -i $web_conf >/var/log/uwsgi/project-web.log 2>&1 &
        echo " done"
    ;;

    stop)
        echo -n "Stopping uWsgi"
        killall -9 uwsgi
        echo " done"
    ;;

    restart)
        $0 stop
        $0 start
    ;;

    show)
        ps -ef|grep uwsgi
    ;;

    *)
        echo -n "Usage: $0 {start|restart|stop|show}"
    ;;

esac
```

####2.2为 uwsgi 脚本设置可执行属性####
```bash executable
chmod +x /etc/init.d/uwsgi
```
####2.3添加 uWsgi 为系统服务（开机自动启动）####
```bash add to sysconfig
chkconfig --add uwsgi 
chkconfig uwsgi on
```
####2.4启动 uWsgi ####
```bash start service
service uwsgi start
```
