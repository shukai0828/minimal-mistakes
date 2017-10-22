---
layout: single
title: "Gitlab中较常见的错误说明及解决"
date: 2014-02-08 16:11:11 +0800
categories: Technology Gitlab
---

即使配置正确的Gitlab在使用时也会经常碰到预料不到的问题，一般需要简单配置调整一下即可。

下面列出我曾经碰到的错误情况及解决办法。

<!--more-->

## error: RPC failed; result=22, HTTP code = 411 ##

提交时本地缓存的容量不足，一般是由于一次性提交了较大的文件或目录，可以通过如下命令解决：

仅设置某一个项目：

    cd /your/path/to/git/project
    git config http.postBuffer 52428800 // 50M，可以改成其他值

全局设置：

    git config --global http.postBuffer 52428800 // 改为最大50M或其他值


## error: RPC failed; result=22, HTTP code = 413 ##

虽然本地可一次性提交的缓存增加了，但是服务器端也需要同时调整。

在服务器端执行如下操作：

1. 打开Gitlab的nginx配置文件，一般是/home/git/gitlab/lib/support/nginx/gitlab_nginx.conf
1. 将配置文件中client_max_body_size的值改为实际需要的值

执行如下命令：

    $ /etc/init.d/nginx reload
    $ su git
    $ /etc/init.d/gitlab restart


其实如果使用SSH方式提交的话，就不需要进行上述的配置了，也不会碰到这个错误提示。

