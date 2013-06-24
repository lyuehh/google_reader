---
layout: post
title:  "FetionRobot 之服务器模式"
date:   2011-10-16 12:37:35
author: 
categories: program
---

## FetionRobot 之服务器模式
### by 
### at 2011-10-16 12:37:35
### original <http://deepfuture.iteye.com/blog/1197700>

FetionRobot 之服务器模式
<br>
<br>最近大家的飞信报警有没有总是发不出来的现象呢,你还在使用命令行来发飞信报警吗.对于发报警数量大的公司来说,可能时不时的飞信就不能用.咨询了移动了解由于飞信频繁的登录导致移动服务器认为恶意登录最后就无法发报警了.有人问能否解决这个问题呢.答案是能,要不我也不会写这个文章了.
<br>
<br> 
<br>
<br>之前大家可能都是使用以下模式来发飞信报警的
<br>
<br> 
<br>
<br>fetion --mobile=你的手机号 --pwd=你的密码 --to=你的手机号 --msg-gb=测试
<br>
<br> 
<br>
<br>这种模式下飞信机器人是发一条短信登陆一次当你的短信量很高时,就会导致飞信无法登陆.移动会要求你更改密码
<br>
<br> 
<br>
<br>所以我们要将飞信机器人更改成服务器模式方法很简单,只需要使用如下命令即可将你的飞信短信在服务器模式下工作了.当你想启动多个手机号的时候只需要再启动相同的命令就可以了注意index号即可
<br>
<br>./fetion --config=mobile.conf --index=1 --hide --daemon --command-path=./commands
<br>
<br> 
<br>
<br>--config=mobile.conf 是一个记录有飞信号和密码的文件内容如下可以配置多个手机号
<br>
<br> 
<br>
<br>    1 152******** mmmm
<br>
<br>    2 150******** mmmm
<br>
<br> 
<br>
<br> 
<br>
<br>1和2 是制定的index号 空格手机号 空格 密码
<br>
<br> 
<br>
<br> 
<br>
<br>--index=1 表示你使用配置文件中的那个飞信号登陆
<br>
<br> 
<br>
<br> 
<br>
<br>--hide 隐身模式
<br>
<br> 
<br>
<br> 
<br>
<br>--daemon 后台 
<br>
<br> 
<br>
<br> 
<br>
<br>--command-path=./commands 这个是关键 这个是发报警短信所需要的目录飞信机器人会监控这个目录下的内容当需要发短信的时候只需要将短信内容追加到这个目录下cmd文件,当飞信机器人发现有符合短信内容的文件就会发送短信将文件删除
<br>
<br> 
<br>
<br> 
<br>
<br>echo &quot;longsms 134******** This is just a test for Jiaion..&quot; &gt; 150********_1.cmd
<br>
<br> 
<br>
<br> 
<br>
<br>我来解释下引号中的内容
<br>
<br>longsms 表示使用长短信 发送 飞信机器人 默认当发送超过100条(我记得是100)的时候会将发送短信中添加广告词语其实只是飞信机器人的论坛地址体谅下作者把人家也不容易
<br>
<br> 
<br>
<br> 
<br>
<br>longsms 后面的手机号134表示将短信发送给谁 
<br>
<br> 
<br>
<br> 
<br>
<br>然后就是短信内容了 
<br>
<br> 
<br>
<br> 
<br>
<br>后面的150表示要使用那个手机号来发送短信这个要和你启动飞信机器人的手机号对应,_1表示一个发送的编号,这里很佩服作者考虑的非常周到希望你使用一个随机数来生成这个号,在发送大量短信的时候会派上用场要不会倒是只会接收到最后一个追加到短信cmd文件中的内容.如果两不大就无所谓了
<br>
<br>
<br>
<br>好了大家可以尽情的享用中国移动给我带来的便利了.也感谢飞信机器人的作者.
<br>
<br>有时候飞信机器人可能会挂到感觉是在每天的夜间.所以别忘记添加飞信机器人监控脚本哦,有人问已经使用daemon模式了还用nohup干嘛 daemon模式有点问题会导致脚本变僵尸
<br>
<br> 
<br>
<br>
<br>
<br>
<br> 
<br>
<br>#!/bin/sh
<br>
<br>date=`date +%F`
<br>cd /AlarmServer/bin/fx/
<br>log= /AlarmServer/bin/fx/log/log_$date
<br>
<br>echo $date &gt;&gt; $log
<br>echo &quot;Check fetion ...&quot; &gt;&gt; $log
<br>
<br>RunFlag=`ps ux | grep "./fetion --config=mobile.conf --index=1" | grep -v grep`
<br>echo $RunFlag &gt;&gt; $log
<br>if [ "AAA$RunFlag" = "AAA" ]
<br>then
<br>echo &quot;fetion1 check failed. start it ...&quot; &gt;&gt; $log
<br>nohup ./fetion --config=mobile.conf --index=1 --hide --daemon --command-path=./commands &amp;
<br>else
<br>echo &quot;fetion1 check ok.&quot; &gt;&gt; $log
<br>fi
<br>
<br>RunFlag=`ps ux | grep "./fetion --config=mobile.conf --index=2" | grep -v grep`
<br>echo $RunFlag &gt;&gt; $log
<br>if [ "AAA$RunFlag" = "AAA" ]
<br>then
<br>echo &quot;fetion2 check failed. start it ...&quot; &gt;&gt; $log
<br>nohup ./fetion --config=mobile.conf --index=2 --hide --daemon --command-path=./commands &amp;
<br>else
<br>echo &quot;fetion2 check ok.&quot; &gt;&gt; $log
<br>fi
<br>
              
              <br><br>
              <span style="color:red">
                <a href="http://deepfuture.iteye.com/blog/1197700#comments" style="color:red">已有 <strong>0</strong> 人发表留言，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
              </span>
              <br><br><br>
<span style="color:#e28822">ITeye推荐</span>
<br>
<ul><li><a href="http://deepfuture.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>