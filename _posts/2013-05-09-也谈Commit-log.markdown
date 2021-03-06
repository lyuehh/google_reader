---
layout: post
title:  "也谈Commit log"
date:   2013-05-09 17:18:55
author: bigwhite
categories: program
---

## 也谈Commit log
### by bigwhite
### at 2013-05-09 17:18:55
### original <http://tonybai.com/2013/05/09/also-talk-about-commit-log/?utm_source=rss&utm_medium=rss&utm_campaign=also-talk-about-commit-log>

<p>在<a href="http://tonybai.com/2011/02/18/put-everything-under-version-control/">版本控制工具</a>大行其道的今天，作为程序员，势必要每天与各种版本控制系统（比如<a href="http://subversion.apache.org">Subversion</a>、<a href="http://git-scm.com">Git</a>、<a href="http://mercurial.selenic.com/%E2%80%8E">Mercurial</a>等）打交道， 每天不commit几次代码都不好意思说自己是专业程序员^_^。不过commit代码可不止敲入commit命令这么简单，对于一个专业程序员 来说，我们还要关注每次commit所携带的背景信息，这里暂且称之为“commit context”。在每次commit时，这些上下文信息只能通过commit log来体现。</p>
<p><b>一、Commit Context</b></p>
<p>今日的软件复杂度日益增加，软件开发模式也早已从单打独斗的英雄模式变成了团队协作模式了，而在团队模式下，版本控制系统发挥着至关重要的作用， 它让开发过程变得有序，将冲突解决的成本尽可能地降低到最低。但版本控制系统毕竟不是智能的，它只是机械地记录着每次提交前后的内容的raw差 异，至于这个差异究竟代表了什么，版本管理系统是不得而知的，这就需要我们开发者们来提供，这就算是产生commit context的动机吧。即便是一个人开发维护的项目，个人的记忆也是有时效性的，时间久了，以前的代码变更context势必也就淡忘了，良好且规范的 commit context有助于更好的维护项目，追踪历史思路和行为，甚至在查找bug时也是能帮得上大忙的，比如确认bug引入的时段边界、代码范围等。</p>
<p>前面说了，commit context最终是以commit log形式提供的，这才是我在这篇文章中真正要说的内容^_^。评价一个项目的好坏，无论是商业项目，还是开源项目，代码本身质量是一个重要的方面，代码 维护的规范性则是另外不可忽略的一个重要因素，而在代码维护规范性方面，commit log的规范是一项重要内容。做了这么多年Coding工作，到目前为止部门内部还没有哪一个项目在commit log规范方面是让我满意和欣赏的。另外本人在亲为commit log方面也是不能让自己满意的，这也是促使我思考commit log这块内容的一个初衷。</p>
<p>commit log承载着每次commit动作的context。一般来说context中至少要有一项内容，那就是此次代码变更的summary，这是最基本的要 求。如果你的commit log还是空着的，那你真该反思反思了，那是对自己和他人的不负责任。但无论是商业公司内部开发还是开源项目，commit context涉及到的因素往往不止一个，很多情况下commit context还与<b>项目过程、质量保证流程以及项目使用的一些工具系统</b>有 关联。我们来看两个知名开源项目的commit log样例吧。</p>
<p><i>[example1 - Linux Kernel]</i></p>
<p><font face="Courier New">audit: catch possible NULL audit buffers<br>
	It&#39;s possible for audit_log_start() to return NULL.  Handle it in the<br>
	various callers.</font></p>
<p><font face="Courier New">Signed-off-by: Kees Cook <a href="mailto:keescook@chromium.org">&lt;keescook@chromium.org&gt;</a><br>
	Cc: Al Viro <a href="mailto:viro@zeniv.linux.org.uk">&lt;viro@zeniv.linux.org.uk&gt;</a><br>
	Cc: Eric Paris <a href="mailto:eparis@redhat.com">&lt;eparis@redhat.com&gt;</a><br>
	Cc: Jeff Layton <a href="mailto:jlayton@redhat.com">&lt;jlayton@redhat.com&gt;</a><br>
	Cc: &quot;Eric W. Biederman&quot; <a href="mailto:ebiederm@xmission.com">&lt;ebiederm@xmission.com&gt;</a><br>
	Cc: Julien Tinnes <a href="mailto:jln@google.com">&lt;jln@google.com&gt;</a><br>
	Cc: Will Drewry <a href="mailto:wad@google.com">&lt;wad@google.com&gt;</a><br>
	Cc: Steve Grubb <a href="mailto:sgrubb@redhat.com">&lt;sgrubb@redhat.com&gt;</a><br>
	Cc: Andrea Arcangeli <a href="mailto:aarcange@redhat.com">&lt;aarcange@redhat.com&gt;</a><br>
	Signed-off-by: Andrew Morton <a href="mailto:akpm@linux-foundation.org">&lt;akpm@linux-foundation.org&gt;</a><br>
	Signed-off-by: Linus Torvalds <a href="mailto:torvalds@linux-foundation.org">&lt;torvalds@linux-foundation.org&gt;</a></font></p>
<p>这是<a href="https://www.kernel.org">Linux Kernel</a>项目的一个commit log的内容。从这个log携带的context信息来看，我们能够清楚地了解如下一些内容：</p>
<p>- 修改的内核模块范围audit<br>
	- 修改的原因summary: to catch possible NULL audit buffers<br>
	- 这个patch从诞生到被merge到trunk过程中涉及到的相关的人员列表<br>
	- 这个patch由Who sign-off的。</p>
<p>将mail list放入到commit log中，这是Linux Kernel开发过程规范所要求的，同样也是质量保证的一个方法。在《<a href="http://tonybai.com/2012/03/27/how-to-participate-linux-community-section-1/">如何加入Linux内核开发社区</a>》系列文章中你可以了解到一些有关Linux Kernel开发过程的内容。从这个例子中我们主要可以看出commit context与Project过程、质量保证链条方面的相关性。</p>
<p><i>[example2 - Apache Subversion]</i></p>
<p><font face="Courier New">Fix issue #3498 – Subversion password stores freeze Eclipse</font></p>
<p><font face="Courier New">* subversion/libsvn_auth_gnome_keyring/gnome_keyring.c<br>
	  (simple_gnome_keyring_first_creds, simple_gnome_keyring_save_creds,<br>
	   ssl_client_cert_pw_gnome_keyring_first_creds,<br>
	   ssl_client_cert_pw_gnome_keyring_save_creds): If the keyring is locked<br>
	    and we are in interactive mode but have no unlock prompt function, don&#39;t<br>
	    throw a &quot;GNOME Keyring is locked and we are non-interactive&quot; error;<br>
	    instead, continue without unlocking it, so that the unlocking may be<br>
	    handled by the default GNOME Keyring unlock dialog box.</font></p>
<p>这是Apache Subversion项目的一个commit log的内容。同样从这个log携带的context信息来看，我们能够清楚地了解如下一些内容：</p>
<p>- 修改的代码范围subversion/libsvn_auth_gnome_keyring/gnome_keyring.c，包括括号中的函数名列表， 这个显然更为细致。<br>
	- 修改的原因summary: <font face="Courier New">Fix issue #3498 – Subversion password stores freeze Eclipse</font><br>
	- 这个patch与问题跟踪系统的关联性 -<font face="Courier New">issue #3498</font>。</p>
<p>通过这个commit log，我们可以快速找到此patch对应的问题跟踪系统中的条目#3498，这样可以查看到一些更为细致的context信息。从这个例子我们主要能够 看出commit context与项目所使用的一些工具系统的关联。</p>
<p>综合以上可以看出良好的commit log是可以清楚全面反映commit context的。这里的“全面”是project-dependent的，是需要能够体现出涉及project的一切必要信息的：过程的、质量的、工具 的。</p>
<p><b>二、Commit log格式</b></p>
<p>Commit log没有放之四海而皆准的统一格式，而是project-dependent的。就我个人而言，我会在下面的几个问题上有纠结。</p>
<p><b><i>* 语言</i></b></p>
<p>不得不承认在创造编程语言方面，西方文化占了主导，语言中的关键字也多取自英语。虽然目前主流的语言以及新兴的语言都号称源码原生支持utf8或 unicode其他字符集格式，但却是很少见到在源文件中使用非英语命名变量或函数的，这也影响了我在commit log中对语言的选择 – 我基本上都是用英文编写commit log的。目前主流的版本控制工具都是支持unicode字符集的，你用中文提交也是没有任何问题的，尤其是在国内商业项目中，使用中文描述起来，理解上快且歧义少。我是不反对用中文写commit log的，但反感的是中英文混合写commit log（有些人用中文，有些人用英文）。每当批量看commit log时，中英文混在一起，一点美感都没有了。</p>
<p>commit log不是给最终用户看的，而是给开发维护人员看的。因此选择语言种类时要看这种语言是否能给开发维护人员的工作带来便利，精确全面地传达context。即便 应用是要发布给非洲人民，但若开发人员都是中国人，一样可以用中文编写commit log。</p>
<p><b><i>* 地道</i></b></p>
<p>说到“地道”，主要是针对你选择<b>外语</b>（大多数情况是英语）作为你commit log的承载语言时。就像生活在国外要用外国人熟悉的语言习惯与人交流似的，我们在用英语编写commit log时也要学会选用“地道”的词汇，远离<a href="http://en.wikipedia.org/wiki/Chinglish">Chinglish</a>。当然想立即做到“地道”也不是那么容易，毕竟我们一直以来就按照Chinglish的思维去学 习英语的，一个比较好的方式就是多看看知名开源项目（比如linux kernel）的commit log，看看人家是如何选择词汇和组织句子的。其实Commit log中用到的词汇和句型很少，看多了也就找猫画虎的学会了。</p>
<p><b><i>* 规范</i></b></p>
<p>“没有规矩，不成方圆”，无论是商业软件项目，还是大型开源项目，莫不如此。如果要想很好的传达commit context，一个设计规范，内容全面的commit log格式是必不可少的。我们无需从头做起，很多开源项目在这方面都已经有一些良好的实践，比如上面提到的linux kernel的commit log convention，再比如这里有Apache Subversion的<a href="http://subversion.apache.org/docs/community-guide/conventions.html#coding-style">Commit log要求</a>。TYPO3和FLOW3也有自己详细的<a href="http://wiki.typo3.org/CommitMessage_Format_(Git)">Commit log说明</a>。</p>
<p>制定规范时总体来说，注意以下几点：<br>
	– 格式简明扼要，只保留必要的项；<br>
	– 注意与项目过程、质量保证流程的结合，以及与第三方工具的关联（注意序号或ID的唯一性）；<br>
	– 对于规模较大的系统，可以考虑在log中体现影响的涉及的“子模块”或“子目录”名字或者逻辑功能的名字（比如前面linux kernel例子中的audit），这样便于快速定位本地commit的影响范畴。</p>
<p><b>三、Commit模板</b></p>
<p>如果像linux kernel或subversion那样涉及到过程、质量控制以及第三方工具的集成（比如问题跟踪系统、代码评审系统等）时，建议设置Commit log template(模板)以简化开发者commit log编写的工作。</p>
<p><b><i>* Subversion命令行客户端支持commit log模板</i></b></p>
<p>Subversion在命令行客户端侧暂无对模板的支持。不过可以通过一些trick模拟实现这个功能：</p>
<p>- 创建commit log模板log.tmpl，放在特定目录下，本例中放在用户的$HOME目录下<br>
	- 添加并导出环境变量SVN_EDITOR<br>
	         <font face="Courier New">export SVN_EDITOR=&quot;rm svn-commit.tmp &amp;&amp; cp ~/log.tmpl svn-commit.tmp &amp;&amp; vi &quot;</font></p>
<p>svn commit时，svn客户端会在当前路径下会执行类似$SVN_EDITOR svn-commit.tmp的命令，而svn-commit.tmp文件已经被替换为我们的模板文件，开发者只需按模板填写内容，并保存退出即可。如果 commit成功，svn客户端会删除当前目录下的svn-commit.tmp，否则svn-commit.tmp不会被删除，这将导致下次再提交 时，svn客户端检测到svn-commit.tmp的存在，从而新建立一个svn-commit.2.tmp的新文件，导致模板失效，这也是这个方法的 一个瑕疵。</p>
<p><b><i>* Git命令行支持commit log模板</i></b></p>
<p>Git是目前very hot的分布式版本管理工具，起步晚，但起点高，因此已经内置了对模板的支持，只需将模板文件配置一下即可。<br>
	        <font face="Courier New"> git config –global commit.template ~/log.tmpl</font></p>
<p><b>四、良好格式commit log</b><b>的实施</b></p>
<p>即便有了良好格式的commit log的模板定义，但就我经验而言，实施起来也还会遇到诸多问题。commit行为是客户端发起的，要让所有开发者都能很好的使用模板并主动按模板提交需 要一些流程以及工具支持。比如在server段部署<a href="http://tonybai.com/2010/08/07/use-svn-pre-commit-hook/">pre-commit hook</a>，对提交的log格式进行检查，不符合模板格式的予以拒绝等。</p>
<p>对于与问题跟踪系统有关联的log格式，还要注意保持问题跟踪系统id或序号的唯一性，这显然是管理和过程方面的工作。</p>
<p>对于开源项目，一般merge到trunk需要owner的检查，所以反倒实施起来容易了些，只要有一篇内容丰富的 developer/community guide或convention之类的文档即可，多数知名的opensource project(比如linux kernel、subversion、apache httpd server、python等)都是有这类文档的，为这些project提交patch前是要好好阅读这些文档的，不能坏了规矩^_^。     <br>
	 </p>
<p style="text-align:left">© 2013, <a href="http://tonybai.com">bigwhite</a>. 版权所有. </p><img src="http://www1.feedsky.com/t1/730350409/bigwhite/feedsky/s.gif?r=http://tonybai.com/2013/05/09/also-talk-about-commit-log/?utm_source=rss&amp;utm_medium=rss&amp;utm_campaign=also-talk-about-commit-log" border="0" height="0" width="0">