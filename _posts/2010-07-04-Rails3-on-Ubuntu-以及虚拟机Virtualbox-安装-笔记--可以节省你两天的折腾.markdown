---
layout: post
title:  "Rails3 on Ubuntu 以及虚拟机Virtualbox 安装 笔记--可以节省你两天的折腾"
date:   2010-07-04 15:10:07
author: 
categories: program
---

## Rails3 on Ubuntu 以及虚拟机Virtualbox 安装 笔记--可以节省你两天的折腾
### by 
### at 2010-07-04 15:10:07
### original <http://www.javaeye.com/topic/705327>

这几天论坛人气不足，灌点水进来。
<br>
<br>
<br>Rails3 on Ubuntu 以及虚拟机Virtualbox 安装 笔记--可以节省你两天的折腾
<br>
<br>永久链接: http://hunter-wxhu-hotmail-com.javaeye.com/blog/705325
<br>
<br>#在windows上安装Ubuntu 虚拟机 
<br>1. download Ubuntu desktop 32 bit (developer to use ubuntu desktop version may is better) 
<br>http://www.ubuntu.com/desktop/get-ubuntu/download 
<br>
<br>2. install virtualbox and set the downloaded ubuntu iso as virtul disk driver and get server installed as step by step 
<br>http://www.virtualbox.org/wiki/Downloads 
<br>设置虚拟机内存为512M，可根据你机器内存来设置 
<br>
<br>3. add another network card for virtual machine in box controller machine, 
<br>关闭虚拟机后，用 box manager给 给虚拟机增加一个网卡（在设置里面） 
<br>具体请细读 
<br>http://muffinresearch.co.uk/archives/2010/02/08/howto-ssh-into-virtualbox-3-linux-guests/ 
<br>
<br>vim /etc/network/interfaces 
<br>
<br>auto eth1 
<br>iface eth1 inet static 
<br>    address 192.168.56.5 
<br>    netmask 255.255.255.0	
<br>
<br>sudo ifup eth1 
<br>
<br>reboot 
<br>
<br>外部有两个网卡，为虚拟机创建，为192.168.56.x字段 
<br>内部也有两个网卡， 一个为自动分配成10.0.2.15字段	
<br>外部网卡一个要设置为nat,一个设置为host-only 
<br>
<br>虚拟机安装要诀： 
<br>虚拟机安装后，要能够从windows xp  host上能够访问到虚拟机， 
<br>本地有虚拟机的两张网卡， 内部有两个网卡， 一个为eth0,一个为eth1 
<br>把eth1设置成和host虚拟机两张网卡同一网段，就可以访问。 
<br>
<br>putty 上去虚拟机后，有时候发现有断掉的情形， 在make的时候，其他的时候还可以。 
<br>
<br>#install ubuntu gnome for server （如果直接安装桌面，就不用了） 
<br>sudo apt-get install x-window-system-core xserver-xorg gnome-desktop-environment 
<br>ref:http://ubuntuforums.org/showthread.php?t=186298 
<br>自动下载要一个小时左右， 安装也要半个小时左右，安装大小在1.5G左右 
<br>download--&gt;uppack--&gt;set up 
<br>
<br>#start vnc server for ubuntu server or desktop 
<br>xterm -geometry 80x24+10+10 -ls -title &quot;$VNCDESKTOP Desktop&quot; &amp; 
<br>gnome-session &amp; 
<br># twm &amp; 
<br>
<br>但是好像装了以后，按s 会有问题 
<br>
<br># 安装一个guest additional  for guest ubuntu 
<br>在虚拟机里面设置光驱为：VBoxLinuxAdditions.iso 
<br>cd /media/cdrom0/ 
<br>sudo ./VBoxLinuxAdditions.run 
<br>
<br>#ubuntu copy/paste with windows host is OK 
<br>using buttion in middle of mouse to copy or just right click to copy 
<br>then paste to windows, windows also can be paste into ubuntu, that is great. 
<br>
<br>#you need install some third party in ubuntu before install ruby 
<br>sudo apt-get zlib1g-dev 
<br>sudo apt-get install libsqlite3-dev 
<br>
<br>#you must need install ruby 1.9.2 Rc1 as I has tryed others will fail as rails3 official required also 
<br>ref =http://www.ruby-lang.org/en/news/2010/07/02/ruby-1-9-2-rc1-is-released/ 
<br>tar xvzf  ... 
<br>$ cd ruby.../ 
<br>$ ./configure 
<br>$ make 
<br>$ sudo make install 
<br>
<br>check ruby version by 
<br>ruby --version 
<br>
<br>#install ruby gems 
<br>ref=http://grigio.org/how_install_rails_3_0_beta_ubuntu_linux_step_step 
<br>wget http://rubyforge.org/frs/download.php/60718/rubygems-1.3.5.tgz 
<br>tar -xvf rubygems-1.3.5.tgz 
<br>cd rubygems-1.3.5 
<br>sudo ruby1.9 setup.rb 
<br>sudo ln -s /usr/bin/ruby1.9.1 /usr/bin/ruby 
<br>sudo ln -s /usr/local/bin/gem /usr/bin/gem 
<br>
<br>sudo gem update --system 
<br>
<br>
<br>#install sqlite3-ruby gem 
<br>sudo gem install sqlite3-ruby 
<br>
<br>#安装thin服务器 
<br>
<br>sudo gem install thin 
<br>
<br># 
<br>#install rails 3 
<br>sudo gem install rails --pre 
<br>it take about 5 minutes to show something, be patient 
<br>total take 10 minutes to install 22 gem related with rails 
<br>
<br>
<br>
<br>#create new rails application 
<br>#rails new bolg 
<br>#cd blog 
<br>
<br>thin start 
<br>
<br>in windows host, you can visit 
<br>http://192.168.56.5:3000/ 
<br>
<br>rails3 welcome is there. 
<br>
<br>安装成功，继续你的网站开发。
          
          <br><br>
          作者: <a href="http://hunter-wxhu-hotmail-com.javaeye.com">山雨欲来风满楼</a> 
          <br>
          声明: 本文系JavaEye网站发布的原创文章，未经作者书面许可，严禁任何网站转载本文，否则必将追究法律责任！
          <br><br>
          <span style="color:red">
            <a href="http://www.javaeye.com/topic/705327" style="color:red">已有 <strong>0</strong> 人发表回复，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
          </span>
          <br><br><br>
<span style="color:#e28822">JavaEye推荐</span>
<br>
<ul><li><a href="http://www.iteye.com/clicks/138"><span style="color:red;font-weight:bold">上海：高薪诚聘Python开发人员</span></a></li><li><a href="http://www.iteye.com/clicks/269"><span style="color:red;font-weight:bold">北京：手机之家网站诚聘PHP程序员</span></a></li></ul>
<br><br><br>