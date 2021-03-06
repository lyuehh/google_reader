---
layout: post
title:  "Javascript实现GMail无刷新文件上传"
date:   2010-10-03 15:32:50
author: Yuanyi ZHANG
categories: program
---

## Javascript实现GMail无刷新文件上传
### by Yuanyi ZHANG
### at 2010-10-03 15:32:50
### original <http://www.letrails.cn/archives/javascript%e5%ae%9e%e7%8e%b0gmail%e6%97%a0%e5%88%b7%e6%96%b0%e6%96%87%e4%bb%b6%e4%b8%8a%e4%bc%a0/>

<p>最近需要给<a href="http://www.51qiangda.com">抢答</a>增加文件上传支持，但是个人不喜欢Flash方式，Google了一下，找到这篇<a href="http://www.openjs.com/articles/ajax/ajax_file_upload/response_data.php">实现异步上传的文章</a>，讲的比较清楚。</p>
<p>要实现类似GMail的无刷新文件上传，原理其实很简单，就是将form的target设置为一个iframe，这样当表单提交后刷新的就是那个iframe，而不是form所在的页面，然后再通过Javascript获取iframe内的响应，并将结果展示给用户。</p>
<p><code><span></span><br>
&lt;%= form_for :photo, :html =&gt; {:id =&gt; &#39;photo_upload&#39;, :multipart =&gt; true, :target =&gt; &#39;upload_target&#39;} do |f| %&gt;<br>
&lt;%= f.file_field :file, {:id =&gt; &#39;file_upload&#39;, <img src="http://www.letrails.cn/wp-includes/images/smilies/icon_surprised.gif" alt=":o"> nchange =&gt; &#39;$(&#39;#photo_upload&#39;).submit();&#39;} %&gt;<br>
&lt;%= end %&gt;<br>
&lt;iframe id=&quot;upload_target&quot; name=&quot;upload_target&quot; style=&quot;width:0;height:0;border:0px solid #fff;&quot;&gt;&lt;/iframe&gt;<br>
</code></p>
<p>就像上面的代码，当用户选择文件后，表单会自动提交，响应会显示在那个隐藏的iframe里面，如果想要告诉用户上传是否成功（比如是否超出尺寸限制等），就需要读取iframe内的数据，可以让Rails返回JSON格式的结果，然后通过JS直接读取。</p>
<p><code><br>
# Rails代码<br>
respond_to do |wants|<br>
if @photo.save<br>
wants.html {render :text =&gt; {:result =&gt; &#39;success&#39;, <img src="http://www.letrails.cn/wp-includes/images/smilies/icon_surprised.gif" alt=":o"> bject =&gt; @photo}.to_json }<br>
else<br>
wants.html {render :text =&gt; {:result =&gt; &#39;failed&#39;, :errors =&gt; @photo.errors}.to_json}<br>
end<br>
end<br>
</code></p>
<p>下面是JS代码，需要为iframe绑定onload方法，这样只有当文件上传结束后，JS才会被调用。</p>
<p><code><br>
$(function(){<br>
$("upload_target").load(function(){<br>
var result = $('#upload_target').contents.find("body").html();<br>
var data = eval("("+result+")");<br>
if(data['result'] != 'success') {<br>
alert("上传成功！");<br>
} else {<br>
alert("上传失败：" + data['errors'][0][1].join());<br>
}<br>
});<br>
});<br>
</code></p>
<p>下面是抢答的Demo，不支持IE，IE请点这里看<a href="http://www.51qiangda.com/forms/4c9b4393b3e57c5591000001">完整表单</a>。</p>
<p> </p>
<table cellspacing="0" cellpadding="3" border="0" style="clear:both">
    
    <tr>
        <td colspan="4"><b><font size="-1" style="display:block!important;padding:20px 0 5px!important">无觅猜您也喜欢：</font></b></td>
    </tr>
    
        <tr>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important">
                    <a title="配置ActionMailer使用GMail发送邮件" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect?url=http%3A%2F%2Fwww.letrails.cn%2Farchives%2Fusing-gmail-smtp-with-actionmailer%2F&amp;from=http%3A%2F%2Fwww.letrails.cn%2Farchives%2Fjavascript%25E5%25AE%259E%25E7%258E%25B0gmail%25E6%2597%25A0%25E5%2588%25B7%25E6%2596%25B0%25E6%2596%2587%25E4%25BB%25B6%25E4%25B8%258A%25E4%25BC%25A0%2F">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.com/images/blogWidget/wordpress_default.gif" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">配置ActionMailer使用GMail发送邮件</font>
                    </a>
                </td>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="Plotr：轻量级的Javascript制图库" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect?url=http%3A%2F%2Fwww.letrails.cn%2Farchives%2F54%2F&amp;from=http%3A%2F%2Fwww.letrails.cn%2Farchives%2Fjavascript%25E5%25AE%259E%25E7%258E%25B0gmail%25E6%2597%25A0%25E5%2588%25B7%25E6%2596%25B0%25E6%2596%2587%25E4%25BB%25B6%25E4%25B8%258A%25E4%25BC%25A0%2F">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.cn/site_images/2011/03/08/3251506.png" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">Plotr：轻量级的Javascript制图库</font>
                    </a>
                </td>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="Aptana IDE: 全新开源Ruby+Rails+JavaScript+HTML工作台" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect?url=http%3A%2F%2Fwww.letrails.cn%2Farchives%2F7%2F&amp;from=http%3A%2F%2Fwww.letrails.cn%2Farchives%2Fjavascript%25E5%25AE%259E%25E7%258E%25B0gmail%25E6%2597%25A0%25E5%2588%25B7%25E6%2596%25B0%25E6%2596%2587%25E4%25BB%25B6%25E4%25B8%258A%25E4%25BC%25A0%2F">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.cn/site_images/2011/03/08/3251499.png" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">Aptana IDE: 全新开源Ruby+Rails+JavaScript+HTML工作台</font>
                    </a>
                </td>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="Javascript on Junction: 客户端的RoR" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect?url=http%3A%2F%2Fwww.letrails.cn%2Farchives%2F28%2F&amp;from=http%3A%2F%2Fwww.letrails.cn%2Farchives%2Fjavascript%25E5%25AE%259E%25E7%258E%25B0gmail%25E6%2597%25A0%25E5%2588%25B7%25E6%2596%25B0%25E6%2596%2587%25E4%25BB%25B6%25E4%25B8%258A%25E4%25BC%25A0%2F">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.com/images/blogWidget/wordpress_default.gif" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">Javascript on Junction: 客户端的RoR</font>
                    </a>
                </td>
        </tr>
    
    <tr>
        <td colspan="4" align="right">
            <a style="text-decoration:none!important" href="http://www.wumii.com/widget/relatedItems" title="无觅相关文章插件">
                <font size="-1" color="#bbbbbb" style="display:block!important;font-family:arial!important;padding:5px 0!important;font-size:12px!important;color:#bbb!important">无觅</font>
            </a>
        </td>
    </tr>
</table>