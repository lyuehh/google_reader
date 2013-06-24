---
layout: post
title:  "Ruby应用几则（解析HTML、XMLRPC）"
date:   2012-08-09 14:26:00
author: Kevin Lynx
categories: program
---

## Ruby应用几则（解析HTML、XMLRPC）
### by Kevin Lynx
### at 2012-08-09 14:26:00
### original <http://www.udpwork.com/item/7908.html>

<h3>Ruby解析HTML</h3>
<p>Ruby解析HTML（或XML）可以使用<a href="http://nokogiri.org/">nokogiri</a>。我的应用里需要查找HTML页面里的某个元素，结果发现实现方式非常简单，就像使用jquery一样。例如我要获取到octopress博客文章里的文章内容、文章标题、文章分类，就像这篇博客：</p>
<div><pre># get post title and content for an octopress post
def post_info(url)
  doc = Nokogiri::HTML(open(url))
  content = doc.css('div.entry-content').to_s
  title = doc.css('header h1.entry-title').inner_html
  categories = doc.css('a.category').collect do |link| link.content end
  return title, content, categories
end
</pre></div>
<p>最关键就是doc.css('div.entry-content')。想起以前用lisp写的那个版本，还手工遍历了整个HTML页面，实在太落后了。上面这个函数的作用就是取得一篇博文的HTML页面，然后返回该博文的内容、标题和分类。</p>
<h3>Ruby调用xml-rpc</h3>
<p>可以使用rails-xmlrpc这个库，直接使用gem安装：gem install rails-xmlrpc。这个库分为客户端和服务器两部分，我的应用是使用metaweblog API：</p>
<div><pre>class MetaWeblogClient &lt; XMLRPC::Client
  def initialize(username, password, host, url)
    super(host, url)
    @username = username
    @password = password
  end

  def newPost(post, publish)
    call(&quot;metaWeblog.newPost&quot;, &quot;0&quot;, &quot;#{@username}&quot;, &quot;#{@password}&quot;, post, publish)
  end

  # other methods

end

def new_post(api, url)
  title, content, categories = post_info(url)
  if title.nil? or content.nil?
    puts &quot;get post info failed at #{url}\n&quot;
    return
  end
  post = { :title =&gt; title, :description =&gt; content, :categories =&gt; categories }
  api.newPost(post, true)
  puts &quot;new post #{title} in #{categories} done\n&quot;
end

api = MetaweblogClient.new(username, password, host, url)
new_post(api, &quot;http://codemacro.com/2012/08/07/write-standalone-ruby-script/&quot;)
</pre></div>
<h3>Ruby读取yaml</h3>
<p>就像Rails里那些配置文件一样，都属于yaml配置文件。我的应用里只需使用简单的key-value形式的yaml配置，就像：</p>
<div><pre>host: www.cppblog.com
url: /kevinlynx/services/metaweblog.aspx
username: kevinlynx
password: xxxxxx
</pre></div>
<p>解析的时候需要使用yaml库：</p>
<div><pre>file = File.open(filename)
cfg = YAML::load(file)
</pre></div>
<p>针对以上配置，YAML::load得到的结果就是一个hash表：</p>
<div><pre>puts cfg[&quot;host&quot;]
puts cfg[&quot;url&quot;]
</pre></div>
<p>以上，我写了一个小工具，可以让我每次在<a href="http://codemacro.com">codemacro.com</a>发表博客后，使用这个工具自动解析生成的文章，然后发表到CPPBLOG上。完整源码可在这个上：<a href="https://gist.github.com/3301662">https://gist.github.com/3301662</a></p>
<p>原文地址：<a href="http://codemacro.com/2012/08/09/ruby-usage-example/">http://codemacro.com/2012/08/09/ruby-usage-example/</a>
<br>
written by<a href="http://codemacro.com">Kevin Lynx</a> posted at<a href="http://codemacro.com">http://codemacro.com</a></p>

			<div style="margin-top:8px;padding:6px 0;border-top:1px solid #3cf">
				<div style="text-align:center;margin:16px 0;padding:6px;border:0px dashed #999;font-family:arial;font-size:26px;font-weight:bold">
	<a href="http://www.udpwork.com/item/7908.html#review_form" title="不喜欢" style="text-decoration:none">
		<img src="http://www.udpwork.com//images/thumb_down24.gif" alt="">
		<span style="color:#f33">0</span>
	</a>
	   
	<a href="http://www.udpwork.com/item/7908.html#review_form" title="喜欢" style="text-decoration:none">
		<img src="http://www.udpwork.com//images/thumb_up24.gif" alt="">
		<span style="color:#3c3">0</span>
	</a>
</div>				<p>
					由 <a href="http://www.udpwork.com/">IT 牛人博客聚合网站(udpwork.com)</a> 聚合
					|
					<a href="http://www.udpwork.com/item/7908.html#reviews">评论: 0</a>
					|
					<a href="http://book.benegg.com/tag/%E7%BC%96%E7%A8%8B?from=udpwork-feed">10000+ 本编程/Linux PDF/CHM 电子书下载</a>
				</p>
			</div>