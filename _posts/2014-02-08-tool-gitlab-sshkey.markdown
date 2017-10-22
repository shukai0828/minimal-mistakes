---
layout: single
title: "错误配置Gitlab后导致SSH失败"
date: 2014-02-08 16:09:32 +0800
categories: Technology Gitlab
---

大家都说通过SSH方式进行Git操作可以省去输入密码的步骤，看网上很多教程说得都很轻松，但是我在实际配置中总是不能成功。使用的是GitLab 6.5.1。

当按照gitlab的教程上产生ssh key后，在GitBash中执行如下命令总是登录失败：

    $ ssh -T git@scmgit.myscm.cn
    git@scmgit.staff.xdf.cn's password:
    Permission denied, please try again.
    git@scmgit.staff.xdf.cn's password:
    Permission denied, please try again.
    git@scmgit.staff.xdf.cn's password:
    Permission denied (publickey,gssapi-keyex,gssapi-with-mic,password).

<!--more-->

使用Git进行clone时也会产生同样的错误：

    Permission denied, please try again.
    Permission denied, please try again.
    Permission denied (publickey,gssapi-keyex,gssapi-with-mic,password).
    fatal: Could not read from remote repository.

    Please make sure you have the correct access rights
    and the repository exists.

在网上找了各种方式，浪费了有两天的时间，才发现在安装时修改了`gitlab-shell/config.yml`中`auth_file`的路径，导致ssh服务不能正确运行。

通过修改该参数的值为默认值：

{% highlight yaml linenos %}
auth_file: "/home/git/.ssh/authorized_keys"
{% endhighlight %}

然后执行如下操作：

    $ su git
    $ mkdir /home/git/.ssh
    $ chmod go-rwx /home/git/.ssh
    $ cp /old/path/to/authorized_keys /home/git/.ssh/authorized_keys
    $ /etc/init.d/gitlab restart

即可使用ssh方式操作gitlab。

## 可能的其他问题 ##

貌似使用host绑定gitlab域名时，ssh方式也会出错。同样的配置，在测试机（绑定host）和正式机（域名解析）上应用时，测试机会返回“欢迎匿名者”的信息：

    $ ssh -T git@scmgit.test.cn
    Welcome to GitLab, Anonymous!

但在正式服务器上则能正常运行：

    $ ssh -T git@scmgit.myscm.cn
    Welcome to GitLab, 楷书!

