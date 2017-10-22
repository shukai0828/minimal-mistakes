---
layout: single
title: "使用Capistrano完成部署"
date: 2014-11-27 18:22:44 +0800
categories: Technology Devops
---

> 原文：[Deploying with Capistrano][0]

[Capistrano][1]是基于Ruby的程序，它提供了一套高级的工具来完成web应用部署。使用Capistrano的最简单的应用方式，可以让你从源代码控制仓库（SVN、Git）通过SSH通道将代码复制到服务器，而且可以在部署之前或之后执行一些额外的动作，比如重启Web服务器、清理缓存、运行数据库迁移工作等等。这个工具也支持同时向多台服务器部署代码。

<!--more-->

Capistrano的很多高级选项本文里不再过多说明，配合这些选项可以部署任何类型的应用。本文只讲解如何使用Capistrano将SVN或Git里的Rails程序部署到单台服务器上，让大家对整个过程的具体步骤有一定了解。这个过程也会把常见的多环境部署流程演示出来：staging和production。

## 安装Capistrano ##

安装Capistrano的电脑上需要有Ruby何RubyGems，OSX10.5+的Mac电脑默认就装好了，否则请参考：[learn how to install Ruby and RubyGems][2]。

然后运行如下命令：

    gem install capistrano

本文中会使用一个更便利的capistrano-ext包，这个gem包含一套额外的工具，使部署过程更简单。

    gem install capistrano-ext

碰到问题的话，请从如下两个链接获取有用的信息：

- [getting started guide][3]
- [find out more about the various components][4]

### 服务器依赖 ###

Capistrano需要SSH通道才能完成使命，因此要保证被部署服务器有SSH服务，执行部署任务的电脑能够执行SSH命令。

参考如下链接建立SSH环境：

- [Mac][5]
- [Windows][6]

## 为Capistrano准备项目 ##

在要被部署项目的根目录执行如下命令：

    capify .

此命令在项目目录下创建一个特殊的文件“*Capfile*”和一个目录“*config*”，*config*目录下有一个*deploy.rb*文件，本文件的初始内容是用部署模板格式化出来的，包含了基本的部署命令。*Capfile*帮助Capistrano正确地加载这个命令模板和相关的库，不过现在暂时还不需要对其进行编辑。

现在用某个文本编辑器打开*deploy.rb*文件，我们所有的部署工作都考这个文件完成！先删除这个文件的所有内容，本文将指导大家填入正确的代码，使我们成功地完成部署步骤的配置。

## 配置Capistrano步骤 ##

> Recipe，这里该如何翻译？暂时用“步骤”表示吧。

### 如何创建Capistrano步骤 ###

在空白的*deploy.rb*文件里输入应用的名称，假设这个应用名称为“fancy shoes”：

{% highlight ruby %}
set :application, "fancy_shoes"
{% endhighlight %}

然后添加项目对应的代码库地址，Git：

{% highlight ruby linenos %}
set :scm, :git
set :repository, "git@account.git.beanstalkapp.com:/account/repository.git"
set :scm_passphrase, ""
{% endhighlight %}

SVN：

{% highlight ruby linenos %}
set :scm, :subversion
set :repository, "https://account.svn.beanstalkapp.com/repository"
{% endhighlight %}

还可以设定在服务器上运行Capistrano命令的用户名称：

{% highlight ruby %}
set :user, "server-user-name"
{% endhighlight %}

确保这个用户具有被部署目录的读写权限，这个目录的路径保存在*deploy.rb*文件的`deploy_to`变量中。

在进行下一步之前，最好在终端里尝试连接一下代码库（`svn co`或`git clone`），保证能够正确鉴权。

现在继续向*deploy.rb*文件里添加服务器的信息，这里我们将使用**Capistrano Multistage**功能（这个功能是**capistrano-ext**包自带的），这样我们就可以使用一个配置文件部署代码到多个位置，本示例将把代码部署到staging和production环境。

在*deploy.rb*文件头部添加如下内容：

{% highlight ruby %}
require 'capistrano/ext/multistage'
{% endhighlight %}

然后指定需要支持的环境的类型，使用变量`stages`：

{% highlight ruby linenos %}
set :stages, ["staging", "production"]
set :default_stage, "staging"
{% endhighlight %}

相比之下，我们向staging环境的部署操作要比production更频繁，将staging设为默认的部署环境更有好处。

然后在*config*目录下建立一个名为*deploy*的目录，在这个目录添加*production.rb*和*staging.rb*文件。Capistrano需要每个被部署的环境都有一个与之同名的Ruby文件，这样才能在部署的时候按照要求加载正确的文件。

现在开始填充*production.rb*的配置：

{% highlight ruby linenos %}
server "my_fancy_server.com", :app, :web, :db, :primary => true
set :deploy_to, "/var/www/fancy_shoes"
{% endhighlight %}

还有*staging.rb*：

{% highlight ruby linenos %}
server "my_fancy_server.com", :app, :web, :db, :primary => true
set :deploy_to, "/var/www/fancy_shoes_staging"
{% endhighlight %}

本例中两个不同的环境使用的是同一个服务器，角色也是相同的（app、web和db），只有`deploy_to`的值不一样，真实情况下两个环境的服务器都应该不同。


### 验证配置的步骤 ###

万事俱备，只欠验证。现在需要Capistrano在服务器上初始化目录结构，为部署做准备。在应用的根目录运行如下命令：

    cap deploy:setup

运行这个命令，Capistrano将会SSH到指定的服务器，进入`deploy_to`变量指向的目录，然后创建一个Capistrano需要的特殊目录结构。出现读写权限或SSH访问权限问题的话，会有报错信息产生，请在命令执行时注意观看它的输出。

在进行真实的部署之前，我们要检查上述命令创建的环境符合要求，运行如下命令校验一下：

    cap deploy:check

这个命令会检查你的本地环境跟服务器环境，定位可能的问题，如果看到了任何错误信息，请修复它们并再次运行此命令，直到没有错误产生，才能继续后面的操作。

### 使用这个步骤文件完成部署 ###

验证通过了，执行如下命令：

    cap deploy

这将会执行默认环境的部署，本示例中设定的默认环境是staging，如果想要部署到production环境，请运行如下命令：

    cap production deploy

执行过程中，会有大量的输出，Capistrano将其在服务器上执行的命令的输出都打印了出来，便于我们调试问题。

## 提示和陷阱 ##

### 用远程缓存提升性能 ###

Capistrano在执行部署时，每次都会将代码库完全clone/export出来，代码规模大的时候会很慢。可以在*deploy.rb*文件里添加带选项的特定命令来提速，请在*deploy.rb*里的`scm`配置部分添加如下内容：

{% highlight ruby %}
set :deploy_via, :remote_cache
{% endhighlight %}

该命令使Capistrano在首次执行部署时完成完整的clone或export，后续的部署将会用`svn up`或`git pull`进行增量更新。

### 添加自定义的钩子 ###

Capistrano不仅仅能够通过SSH通道复制文件，我们可以配置一些额外的事件和命令，比如重启WEB服务器、运行一个脚本等，这些事件和命令在特定的场合下执行，比如在文件复制完成后。Capistrano将这些事件或命令统称为“任务”，举个例子，请将如下内容复制到*deploy.rb*文件中：

{% highlight ruby linenos %}
namespace :deploy do
  task :restart, :roles => :web do
    run "touch #{ current_path }/tmp/restart.txt"
  end

  task :restart_daemons, :roles => :app do
    sudo "monit restart all -g daemons"
  end
end
{% endhighlight %}

Capistrano的任务功能很强，本文浅尝辄止，我们可以创建各种在服务器上执行操作的任务，设定它们在部署前、部署后或独立于部署行为之外实际执行。任何类型的维护工作都可以：重启进程、清除文件、发送邮件通知、运行数据库迁移、执行脚本等等。

这个例子里包含两个自定义的任务，“restart”是Capistrano的内置任务，会在部署工作完成后被自动执行，这里使用的`touch tmp/restart.txt`技术是Rails应用经常使用的方法，针对你的WEB服务器可能需要其它不同的命令。

第二个示例任务“restart_daemons”在默认情况下不会被Capistrano执行，为了使它得到运行，需要在*deploy.rb*里增加一个钩子调用：

{% highlight ruby %}
after "deploy", "deploy:restart_daemons" 
{% endhighlight %}

该命令告知Capistrano在所有部署操作完成后执行设定的任务，还有其它的一些钩子，包括在复制文件之前执行的“before”类型。

可以在官方站点获得更多有关after和before的钩子信息：

- [Before Tasks][7]
- [After Tasks][8]

### 将Git分支同部署环境关联 ###

一般情况下，我们想要把这里使用的两个服务器环境（staging和production）跟Git里对应这些环境的分支绑定到一起，这样就可以自动地部署staging分支到staging环境、部署master分支到production环境。只需要在*production.rb*文件里添加如下内容：

{% highlight ruby %}
set :branch, 'master'
{% endhighlight %}

在*staging.rb*文件里添加如下内容：

{% highlight ruby %}
set :branch, 'staging'
{% endhighlight %}

现在，每当我们运行`cap deploy`时，Capistrano将从staging分支部署代码到staging环境；运行`cap production deploy`将会从master分支部署代码到production环境。

易如反掌，对不对。

## 延伸阅读 ##

- [Capistrano官方Wiki][9]
- [Capistrano！从头开始][10]



[0]: http://guides.beanstalkapp.com/deployments/deploy-with-capistrano.html
[1]: http://www.capistranorb.com/
[2]: http://rubyonrails.org/download
[3]: https://github.com/capistrano/capistrano/wiki/2.x-Getting-Started
[4]: https://github.com/capistrano/capistrano/wiki/2.x-From-The-Beginning
[5]: http://guides.beanstalkapp.com/version-control/git-on-mac.html#creating-ssh-keys
[6]: http://guides.beanstalkapp.com/version-control/git-on-windows.html#installing-ssh-keys
[7]: https://github.com/capistrano/capistrano/wiki/2.x-DSL-Configuration-Tasks-Before
[8]: https://github.com/capistrano/capistrano/wiki/2.x-DSL-Configuration-Tasks-After
[9]: https://github.com/capistrano/capistrano/wiki
[10]: https://github.com/capistrano/capistrano/wiki/2.x-From-The-Beginning
