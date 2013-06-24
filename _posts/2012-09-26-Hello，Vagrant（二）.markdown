---
layout: post
title:  "Hello，Vagrant（二）"
date:   2012-09-26 10:26:00
author: dreamhead
categories: program
---

## Hello，Vagrant（二）
### by dreamhead
### at 2012-09-26 10:26:00
### original <http://dreamhead.blogbus.com/logs/222701077.html>

<p>好吧，我承认在<a href="http://dreamhead.blogbus.com/logs/222436900.html">第一部分</a>介绍的那个虚拟机只是看上去很美，因为里面什么都没有，所以，我们必须动手给它装些东西，才能让它有点作用。</p>
<p>别急别急，我知道，你会和最初的我一样，一听到装东西，立刻就要vagrant ssh上去，然后，apt-get install各种各样的东西。可我们是程序员，我们才不做这种普通用户才做的事情呢！</p>
<p>在这个年代，只说持续集成，我们都不好意思了，要说持续交付，而要达到持续交付，有一个叫做DevOps的概念是绕不过去，说白了，DevOps就是把配置环境的过程代码化，所谓infrastructure as code。在这个领域，有两个工具很好用，一个叫Chef，另一个叫Puppet。它们都可以极大程度上简化我们配置环境的过程。</p>
<p>谢天谢地，Vagrant二者都支持。</p>
<p>下面我们用Chef搭建一个CI，体验所谓的DevOps。</p>
<p>Chef支持两种运行模式，Chef Solo和Chef Server。所谓Solo就是一个人的工作，而Server自然是服务器的意思。之所以要有这样的区分，主要是考虑用来配置机器的脚本放在哪里，Solo就是放在自己的机器上，而Server则是放在一台特定的服务器上。在我们这个简单的例子里，就不麻烦Server了。</p>
<p>我们要配置机器，首先要有的就是配置文件，对应的配置文件在Chef中称为cookbook，这个社区里已经很多写好的Cookbook，让我们拿过来用。</p>
<p>  http://community.opscode.com/cookbooks</p>
<p>就我们这个例子里，我们选用的CI软件是Jenkins，所以，我们需要jenkins的cookbook。在这个例子里，我们就在之前建立目录创建一个cookbooks的目录，用以存放我们的cookbook，进入到这个目录下，执行下面的命令：</p>
<p>  git clone https://github.com/heavywater/chef-jenkins jenkins</p>
<p>先别急，事实上，仅有一个jenkins是不够的，它还要依赖于其它的程序包，以这个例子而言，我们采用Ubuntu操作系统，所以，我们需要安装apt，再有jenkins是一个Java编写的应用，所以，它还需要一个java：</p>
<p>  git clone https://github.com/opscode-cookbooks/apt.git</p>
<p>  git clone https://github.com/opscode-cookbooks/java.git</p>
<p>好，我们要用的cookbook都有了，下面该让Vagrant知道它们的存在了。上次说道，我们在init之后，会生成一个Vagrantfile。实际上，它就是Vagrant的配置文件。用你最喜欢的编辑器打开它。</p>
<p>实际上，关于如何配置Vagrant，主要内容都在里面了，只是大部分都注释掉了而已。既然我们要采用Chef Solo模式，那就找到相关的配置，搜索chef_solo，很容易定位到，将它改成下面的样子：</p>
<p>  config.vm.provision :chef_solo do |chef|<br>    chef.cookbooks_path = &quot;cookbooks&quot;<br>    chef.add_recipe &quot;jenkins&quot;<br>  end</p>
<p>这个配置是告诉Vagrant，cookbook的路径在哪，以及我们要用的哪个配置。</p>
<p>这样，基本的配置就是这样了，但是，我们虽然启动了这个jenkins，却只能在虚拟机内部访问，为了让别人也能访问，我们需要配置转发端口，同样在Vagrantfile里：</p>
<p>  config.vm.forward_port 8080, 7070</p>
<p>这个配置是说，虚拟机内部的8080端口会转发到宿主端口7070。</p>
<p>等了半天，启动我们的虚拟机吧，还记得命令吗？</p>
<p>  vagrant up</p>
<p>如果你之前启动过，为了让新配置生效，我们可以：</p>
<p>  vagrant reload</p>
<p>虽然是自动安装，但漫长的安装过程是无法避免的，可以先休息一下。</p>
<p>幸运的话，等待之后，我们就可以打开我们的浏览器，访问新CI了：</p>
<p>  http://localhost:7070</p>
<p>好了，我们的CI就绪了，按照你的需求配置吧！</p>