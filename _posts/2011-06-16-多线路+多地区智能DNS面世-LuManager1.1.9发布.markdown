---
layout: post
title:  "多线路+多地区智能DNS面世 LuManager1.1.9发布"
date:   2011-06-16 10:22:47
author: 
categories: program
---

## 多线路+多地区智能DNS面世 LuManager1.1.9发布
### by 
### at 2011-06-16 10:22:47
### original <http://www.cnbeta.com/articles/145940.htm>

<div><a rel="nofollow" href="http://www.cnbeta.com/topics/128.htm"><img src="http://img.cnbeta.com/topics/global.png" alt="网络软件" name="sign" align="right"></a>
        <p><b>感谢<a rel="nofollow" href="http://www.zijidelu.org">自己的路</a>的投递</b><br>
新闻来源:自己的路<br>
LuManager(LUM)是基于FreeBSD、Zijidelu、Debian、Centos、Ubuntu等Linux/Unix系统的网站服务器管理软件，是目前国内市场上为数不多的同时支持Linux和Unix的网站服务管理软件<strong>。LUM1.1.9增加的重要功能</strong>：集成了包括电信、网通、移动、铁通、电信_华北地区、电信华南地区、网通_华东地区等21条线路任意组合的智能DNS解析模块，如下图所示：</p>
		<p>LuManager(LUM)是基于FreeBSD、Zijidelu、Debian、Centos、Ubuntu等Linux/Unix系统的网站服务器管理软件，是目前国内市场上为数不多的同时支持Linux和Unix的网站服务管理软件（好像是唯一的？）。只要您会上网，就可以搭建和管理Linux/Unix服务器！完美支持Discuz、Phpwind、Shopex、ECShop、Ecmall、Wordpress、Dedecms、PHP168、CmsTop、Magento、Zen-Cart、Xweibo、Iweibo等常用php+mysql结构程序。还集成了常用程序的快速安装方法，如Discuz、Phpwind、ECShop、Dedecms、Xweibo、Iweibo等，安装好LuManager后，5秒内即可创建一个网站！使创建网站的门槛大大降低。LUM是从已经有6年历史的FAMP分离出来的控制面板的升级版本，我们将其命名为LuMananger，即Linux/Unix的管理者，简称LUM。<br>
<br>
<strong>LUM1.1.9增加的重要功能</strong>：<br>
1. 集成了包括电信、网通、移动、铁通、电信_华北地区、电信华南地区、网通_华东地区等21条线路任意组合的智能DNS解析模块，如下图所示：<br>
<img src="http://img.cnbeta.com/newsimg/110616/1022470235554601.png" height="436" width="600"><br>
<br>
2. 跟DedeCMS官方合作，集成了最新的DedeCMS的GBK、UTF8、Big5版本的快速安装方法（5秒内即可安装软件，包括Discuz、Phpwind、EcShop等知名程序都可以快速安装）。<br>
<strong>演示：</strong><a rel="nofollow" href="http://demo.zijidelu.org:8888/"><strong>http://demo.zijidelu.org:8888</strong></a><strong>（用户名：zijidelu，密码：zijidelu）</strong><br>
<br>
<strong>######------ 主要功能包括：</strong><br>
负载均衡集群，CDN静态内容加速，多线路多地区智能DNS解析，云备份，多用户，多用户组，自由切换Apache和Nginx，在线安装微博/论坛/商城等软件，批量上传，批量删除，在线解压，网站和FTP流量限制，流量统计图表，SSL证书，301和302转向，防盗链，网站健康状态查看等。详细说明如下：<br>
<br>
<strong>#-------- 负载均衡 </strong><br>
当网站访问量过大时，就需要多台服务器同时提供服务，LUM的负载均衡功能可以将多台服务器组合成一个高承载高访问量的集群，是千万级以上访问量网站的解决方案。支持后端机器宕机时自动剔除，按后端机器性能分配等高级功能。<br>
<br>
<strong>#-------- CDN静态内容加速 </strong><br>
提供静态内容加速功能，让不同地区或线路的用户访问不同的服务器，是下载或镜像网站的解决方案（创建一个镜像网站是几秒钟的事情）。最简单的应用：可以非常轻松实现让电信用户访问电信服务器，网通用户访问网通服务器，老外就访问放在国外的服务器...可以增加任意多台服务器。支持更新缓存，用json数据格式返回删除结果。由于LUM自带了网站监控和流量统计功能，所以可以使用LUM搭建CDN服务器，对企业提供CDN服务。<br>
<br>
<strong>#-------- 多线路多地区智能DNS </strong><br>
智能DNS能自动判断访问者的IP地址并解析出对应的IP地址，最常见的应用就是使网通用户访问网通服务器，电信用户访问电信服务器。 LUM的智能DNS支持电信、网通、教育网、移动、铁通等多达21条线路的任意组合，可以说是无限种连接方案。结合LUM的CDN功能，可轻易搭建支撑千万级访问量的服务器环境。 <br>
<br>
<strong>#-------- 云备份网站 </strong><br>
将网站数据经过高强度压缩和加密，然后备份至远端服务器。支持多点备份和增量备份，是一种代替FTP和SSH备份的更稳定更可靠的备份方案。 <br>
<br>
<strong>#-------- Nginx与Apache自由切换 </strong><br>
每个网站都可以自由选择以下三种模式：1.完全使用Nginx，2.完全使用Apache，3.仅PHP用Apache处理（即前台Nginx，后台Apache）。并且可以选择是否使用cgi处理PHP，杜绝502错误。<br>
<br>
<strong>#-------- 在线安装常用软件 </strong><br>
可在线安装Discuz/Phpwind/EcShop/EcGroupon/Xweibo/iWeibo等程序，会上网就会建网站！<br>
<br>
<strong>#-------- 安全可靠，防止跨站攻击 </strong><br>
不论使用的是Apache还是Nginx，都不可以跨目录访问别的网站，当服务上的一个网站出现问题，不会危及整台服务器上的数据。进行危险操作时需提交保护密码；防止远程提交form；验证码需要点击才能显示等（不能通过机器暴力登陆，只能通过人工登陆）。<br>
<br>
<strong>#-------- 网站锁 </strong><br>
可以将网站锁住，就算网站程序有漏洞，也不会被挂木马或中病毒。<br>
<br>
<strong>#-------- 多用户 </strong><br>
每个用户都可以无限级添加自己的子用户，并且任何一个用户都可以成为超级管理员（更改config.php文件）。就像鸡生蛋，蛋再生鸡和寡蛋...<br>
<br>
<strong>#-------- 多用户组 </strong><br>
每个用户都可以拥有多个用户组，并为子用户指定用户组，然后可以通过用户组控制子用户的权限。LUM的权限控制可以精确到具体行为，如可禁止某用户是否有使用301转向功能，是否有重启服务器的功能等。权限控制是递归的，如果某个父用户没有关机的权限，那么其所有子用户都没有关机的权限了（可见越后端的用户，权限越小）。<br>
<br>
<strong>#-------- SSL证书支持 </strong><br>
可以用LUM直接生成ssl证书和证书申请文件，生成后马上就可以用（测试站点），任何人都可以创建支持SSL证书的站点，不需再用命令去操作。<br>
<br>
<strong>#-------- 文件管理器 </strong><br>
能直接对文件或目录进行复制、上传、下载、删除、编辑、压缩、解压等操作，支持批量上传，批量删除，高强度加密压缩，支持分卷压缩。<br>
<br>
<strong>#-------- 网站监控 </strong><br>
可以看到每个网站的连接数、请求总数、成功率等信息<br>
<br>
<strong>#-------- 301和302转向 </strong><br>
可以设置301永久转向和302临时转向。<br>
<br>
<strong>#-------- 网站流量限制 </strong><br>
可以设置每个访客的最大线程和最大的游览网页的速度<br>
<br>
<strong>#-------- 支持Linux和Unix系统 </strong><br>
目前已经支持的系统有FreeBSD（Unix系），Debian，Ubuntu系列（Linux mint, 深度Linux，YLMF Linux，KUbuntu等），Redhat系列（RHEL, CentOS等）...最重要的，LUM不会破坏系统的安全保护机制，请放心使用！<br>
<br>
<strong>#-------- 强大的流量统计功能 </strong><br>
能统计网站的最近5分钟，10分钟，半小时，3天，7天、10天、180天，本月，本年等时间段流量。每天0点30分自动统计，支持日志回滚和自动切割，不需担心日志过大的问题。并且可以生成流量统计图表。<br>
<br>
<strong>#-------- 支持套餐定制 </strong><br>
可以对产品（FTP，虚拟主机，数据库）进行集中管理，如增加FTP的下载速度，限制FTP大小，控制FTP、网站、数据库的个数等，接下来还将开发支持限制资源使用的功能。<br>
<br>
<strong>#-------- FTP，主机，数据库分离 </strong><br>
一个FTP下可以建N个网站，一个网站可以连接任意多个数据库，数据库和网站只相关不相连。一个用户可以有N个FTP，一个FTP下又可以有N个网站。<br>
<br>
<strong>#-------- 反向代理 </strong><br>
只需填写被代理网址，别的都交给LUM去做，而且可以为代理网站开启静态缓存。<br>
<br>
<strong>#-------- 身份验证 </strong><br>
可以为网站增加一个访问用户名和密码，只有通过验证的用户才能访问网站。<br>
<br>
<strong>#-------- 可选择常用软件的伪静态规则 </strong><br>
包括Discuz、PhpWind、ShopEX、Wordpress等常用程序的伪静态规则。<br>
<br>
<strong>#-------- Memcached缓存管理 </strong><br>
在线启动和关闭Memcached，并可设置所使用的内存大小，连接数，是否开机启动等。<br>
<br>
<strong>#-------- 在线更改系统密码 </strong><br>
我们最终的目标是实现让用户不必懂任何一个命令即可管理成百上千台服务器，我们离目标又近了一步。<br>
<br>
<strong>#-------- 保护密码 </strong><br>
每个用户都可以设置保护密码，如果没有保护密码，就算登陆LUM控制面板，也不能进行危险操作。<br>
<br>
<strong>#-------- 禁止直接访问phpMyAdmin </strong><br>
可以在后台关闭phpMyAmin的直接访问，只能通过LUM后台才能访问phpMyAmin。<br>
<br>
<strong>#-------- 禁止root用户远程登陆系统 </strong><br>
还想通过暴力行为登陆SSH？没门！<br>
<br>
<strong>#-------- 防盗链 </strong><br>
不仅可以设置允许链接的域名，还可以设置被盗链时的默认图片。<br>
<br>
<strong>#-------- 备份与还原 </strong><br>
支持对网站，数据库或者普通文件夹直接打包备份或者解压还原，支持zip, tar.gz, bz2等压缩格式。<br>
<br>
<strong>#-------- 操作系统行为控制 </strong><br>
支持在后台直接重启，关机，Nginx，Apache，MySQL，Pure-ftpd等软件的重启，重载，关闭等功能。<br>
<br>
<strong>#-------- 操作FTP和网站的文件权限互通系统 </strong><br>
开通网站后，能过FTP上传程序即可使用，不需改文件权限。通过FTP上传的文件在网站程序中可以编辑或删除，网站生成的文件在FTP中也能编辑或删除。而且可在编辑虚拟主机时一次性将网站所有文件更改成777、775、644等权限。<br>
<br>
<strong>#-------- 在线下载远程数据 </strong><br>
填入远程文件的网址，轻轻一点便可以将远程文件下载，然后还可以解压，然后还可以移动...<br>
<br>
<strong>#-------- 错误页控制 </strong><br>
支持在后台控制403、404、500、501、502错误页<br>
<br>
<strong>#-------- FTP和网站目录自由定制 </strong><br>
可以将FTP定在/home/以外的分区。<br>
<br>
<strong>#-------- 在线编辑配置文件 </strong><br>
可在后台编辑php.ini、my.cnf、httpd.conf、nginx.conf、pure-ftpd.conf等文件。<br>
<br>
<strong>#-------- 支持Nginx和Apache扩展设置 </strong><br>
在后台便可以操作httpd和nginx.conf文件内容，不必用ssh操作。<br>
<strong>#-------- 可查看程序进程和版本信息 </strong><br>
在后台首页可以查看程序运行时的状态，包括进程数，程序版本等。<br>
<br>
<strong>#-------- 硬件信息 </strong><br>
可查看CPU，硬盘，内存的等硬件信息，查看内存和硬盘的使用情况，可对服务器的性能进行评分，让您对服务器的性能了如指掌。<br>
<br>
<strong>#-------- 漂亮和人性化的操作界面 </strong><br>
大量采用ajax无刷新技术，不仅简便，而且更符合操作习惯，操作起来就像在操作桌面应用程序。<br>
<br>
<strong>#-------- 增加保护目录 </strong><br>
管理员可以将常用的伪静态文件存放在/home/lum_safe_files的保护目录下，升级了LUM后不丢失。<br>
<br>
<strong>#-------- 在线升级 </strong><br>
具有指纹验证功能的在线升级功能，确保您不会因官方域名被劫持而升错文件<br>
<br>
<strong>#-------- 可为每个网站定制Nginx的http段和server段的扩展内容 </strong><br>
再也不必通过vi来编辑nginx配置文档了。<br>
<br>
<strong>######------ 下载和安装说明：</strong><br>
1. 安装系统：最小化安装系统（最好是全新安装系统，VPS用户可跳过此步骤，也可以在VPS控制面板中重装系统。如果是实体机用户，我们建议您选择<a rel="nofollow" href="http://www.zijidelu.org/thread-1734-1-1.html">ZijideluOS</a>）。<br>
系统下载：<br>
<a rel="nofollow" href="http://www.zijidelu.org/thread-1336-1-1.html">http://www.zijidelu.org/thread-1336-1-1.html</a><br>
<a rel="nofollow" href="http://www.zijidelu.org/thread-45-1-1.html">用最小化方式安装FreeBSD的图解教程</a>。<br>
<a rel="nofollow" href="http://www.zijidelu.org/thread-317-1-1.html">Debian最小化安装图文教程</a><br>
<a rel="nofollow" href="http://v.youku.com/v_show/id_XMjAwMTg3NjEy.html">CentOS 5.4 最小化安装视频教程</a><br>
<a rel="nofollow" href="http://www.zijidelu.org/thread-1793-1-1.html">使用LuManager时的分区建议</a><br>
<a rel="nofollow" href="http://www.zijidelu.org/thread-2099-1-1.html">在VPS上安装LuManager的视频教程</a><br>
<br>
2. 登陆系统：用超级用户root登陆系统。如果是FreeBSD，请切换到/home目录（cd /home，如果没有/home目录，则切换到/usr目录）。我们建议您用ssh工具并设置成UTF-8编码登陆服务器，因为那样可以看到中文提示。SSH工具用putty，设置UTF-8编码的方法见<a rel="nofollow" href="http://www.zijidelu.org/thread-1818-1-1.html">http://www.zijidelu.org/thread-1818-1-1.html</a>。<br>
3. 下载zijidelu_install.sh安装脚本：wget <a rel="nofollow" href="http://down.zijidelu.org/zijidelu_install.sh">http://down.zijidelu.org/zijidelu_install.sh</a>（FreeBSD系统则用fetch代替wget）<br>
4. 赋予zijidelu_install.sh可执行权限：chmod 750 zijidelu_install.sh（或者chmod u+x zijidelu_install.sh）<br>
5. 安装：<strong>./zijidelu_install.sh -y</strong>（此为全自动安装，如需半自动安装，请不用-y参数，即：<strong>./zijidelu_install.sh</strong>）<br>
6. 选择下载点：China的为中国电信下载点（默认），USA为美国下载点。选择好后回车<br>
7. 选择您要安装的版本：选1为1.0.X，选2为1.1.X（如果是Redhat系列系统，会提示您是否用rpm包来快速安装LuNamp，建议时间比较紧或编译安装失败的用户选择rpm包进行安装）</p></div>