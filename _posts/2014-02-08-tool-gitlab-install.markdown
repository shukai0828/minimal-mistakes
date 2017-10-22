---
layout: single
title: "Gitlab的安装和配置-CentOS"
date: 2014-02-08 16:09:10 +0800
categories: Technology Gitlab
---

[GIT][0]作为新兴的代码管理工具异军突起，它的很多特性让开发人员兴奋不已，基于git的代码托管服务也在蓬勃发展，最知名的要数[Github][1]，国内也有一些知名网站基于[Gitlab][2]提供了自己的代码托管服务。Github和Gitlab提供的代码在线浏览、共享为技术人员提供了便捷的“社交”化功能。

不过对于很多技术公司来说，将代码托管在这些云端毕竟有很多不便，还是期望能够搭建自己的GIT代码托管服务，幸而Gitlab是开源的平台，可以用来完成自己的目标。

Gitlab官方免费提供基于Debian系统的支持，我们公司使用的CentOS操作系统，因此在参考网上的一些指导后，终于成功安装了自己的Gitlab。

安装和配置过程中也出现了比较的问题，虽然最终都解决了，但是走了较多弯路，浪费了一些时间，这里把整个过程记录下来，供以后备忘。

<!--more-->

## 操作系统 ##

操作系统是CentOS 6 x86_64

    $ uname -a
    Linux localhost.localdomain 2.6.32-358.el6.x86_64 #1 SMP Fri Feb 22 00:31:26 UTC 2013 x86_64 x86_64 x86_64 GNU/Linux

## 依赖的软件 ##

* EPEL
* Nginx 1.4.4
* Ruby 1.9.3+
* Python 2.7+
* Redis 2.0+
* git 1.7.10+
* MySQL // 也可使用其他主机的MySQL服务
* [ICU][3]

上述软件建议找系统管理员协助安装。

## 安装 Git 和 Gitolite ##

    $ yum -y install git-all gitolite

## 创建Gitlab数据库 ##

配置 gitlab 需要的用户和数据库，假设Gitlab安装在服务器192.168.25.18上。

    $ mysql> CREATE USER 'gitlab'@'192.168.25.18' IDENTIFIED BY 'somepasswd';
    $ mysql> CREATE DATABASE `gitlabhq_production`;
    $ mysql> GRANT * ON `gitlabhq_production`.* TO '192.168.25.18';

## 添加 Gitlab 用户 ##

    $ useradd -c 'GitLab' git
    $ passwd -l git

## 安装 Gitlab-shell ##

    $ su git
    // 后续操作若无额外说明，都在git权限下进行
    $ cd /home/git
    $ git clone https://github.com/gitlabhq/gitlab-shell.git
    $ cd gitlab-shell

查看可用的Gitlab-shell版本：

    $ git tag
    v1.0.3
    v1.1.0
    ...
    v1.7.7
    v1.7.8
    v1.7.9
    v1.8.0

这里我们选择最新的版本1.8.0：

    $ git checkout v1.8.0 

编辑配置文件，修改`gitlab_url`的值为自己想要的域名，比如：http://git.example.com，这里还可以修改redis的配置。

这个配置文件里有一个参数`auth_file`，不用试图进行修改其路径，我在修改了之后，导致后来git的ssh方式不能正常使用，而且好像`gitlab_url`对应的域名需要做host绑定的话也会导致ssh方式不能正常工作。

    $ cp config.yml.example config.yml
    $ vi config.yml

执行安装

    $ ./bin/install

## 安装 Gitlab ##

    $ cd /home/git
    $ git clone https://github.com/gitlabhq/gitlabhq.git gitlab
    $ cd /home/git/gitlab

查看可用的Gitlab版本。

    git tag
    v1.0.1
    v1.0.2
    v1.1.0
    ...
    v6.4.1
    v6.4.2
    v6.4.3
    v6.5.0
    v6.5.0.rc1
    v6.5.1

选择最新的版本6.5.1。

    $ git checkout v6.5.1 

修改配置文件，主要修改如下字段：

{% highlight yaml linenos %}
host: git.example.com
email_from: scm@gitxdf.cn
path: /path/to/your/git/gitlab-satellites/
repos_path: /path/to/your/git/repositories/
{% endhighlight %}

    $ cd /home/git/gitlab
    $ cp config/gitlab.yml.example config/gitlab.yml
    $ vi config/gitlab.yml

创建一系列目录，不明白为什么gitlab不默认创建这些目录：

    $ mkdir tmp/pids/
    $ mkdir tmp/sockets/
    $ mkdir public/uploads
    $ mkdir /path/to/your/git/gitlab-satellites

修改 unicorn 配置文件，主要修改这个字段：

* timeout 300 // 第一次执行时，初始化需要较长的时间

在这里修改：

    $ cp config/unicorn.rb.example config/unicorn.rb
    $ vi config/unicorn.rb

设置一些 git 全局参数：

    $ git config --global user.name "GitLab"
    $ git config --global user.email "gitlab@localhost"
    $ git config --global core.autocrlf input

配置 gitlab 数据库设置，我们使用production配置，只需要保证production的参数配置正确即可：

{% highlight yaml linenos %}
production:
  adapter: mysql2
  encoding: utf8
  reconnect: false
  database: gitlabhq_production
  pool: 10
  username: git
  password: "mypassword"
  host: 192.168.25.100
  # socket: /tmp/mysql.sock
{% endhighlight %}

    $ cp config/database.yml.mysql config/database.yml
    $ vi config/database.yml

## 安装 Ruby Gems ##

    $ cd /home/git/gitlab
    $ gem install charlock_holmes --version '0.6.9.4'
    $ bundle install --deployment --without development test postgres aws

初始化数据库：

    $ bundle exec rake gitlab:setup RAILS_ENV=production

设置 init 脚本（需要root权限）：

    $ exit // 退出git账户
    $ cp lib/support/init.d/gitlab /etc/init.d/gitlab
    $ chmod +x /etc/init.d/gitlab

检查 Gitlab 状态：

    $ bundle exec rake gitlab:env:info RAILS_ENV=production
    System information
    System:		CentOS 6.4
    Current User:	root
    Using RVM:	yes
    RVM Version:	1.25.15
    Ruby Version:	2.1.0p0
    Gem Version:	2.2.1
    Bundler Version:1.5.2
    Rake Version:	10.1.0

    GitLab information
    Version:	6.5.1
    Revision:	6f6f158
    Directory:	/home/git/gitlab
    DB Adapter:	mysql2
    URL:		http://scmgit.example.cn
    HTTP Clone URL:	http://scmgit.example.cn/some-project.git
    SSH Clone URL:	git@scmgit.example.cn:some-project.git
    Using LDAP:	no
    Using Omniauth:	no

    GitLab Shell
    Version:	1.8.0
    Repositories:	/path/to/your/git/repositories/
    Hooks:		/home/git/gitlab-shell/hooks/
    Git:		/usr/bin/git

启动 gitlab 服务，需要修改git家目录的权限，否则不能正常提供服务：

    // 使用root权限
    $ chmod o+x /home/git
    $ sudo service gitlab start

再检查一下启动后的服务状态，这时可能会检查到一些问题：

     $ bundle exec rake gitlab:check RAILS_ENV=production

## 配置 nginx ##

首先需要把gitlab为nginx准备的conf文件做出来：

    // 需要root权限
    $ cp lib/support/nginx/gitlab /etc/nginx/conf.d/gitlab.conf

将新的gitlab.conf路径添加到nginx加载的配置文件里，我的机器里是这个样子的：

{% highlight nginx linenos %}
user  nginx;
worker_processes  1;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/default.conf;
    include /etc/nginx/conf.d/gitlab.conf; // i am here
}
{% endhighlight %}

然后保存并重启各个服务：

    // 使用root权限
    $ /etc/init.d/nginx reload
    $ /etc/init.d/gitlab restart

至此配置完毕，可以在浏览器上进行访问。

默认的用户名密码：

    admin@local.host
    5iveL!fe


[0]: http://git-scm.com "GIT"
[1]: http://github.com "GitHub"
[2]: http://gitlab.org "GitLab"
[3]: http://www.linuxfromscratch.org/blfs/view/svn/general/icu.html "International Components for Unicode"