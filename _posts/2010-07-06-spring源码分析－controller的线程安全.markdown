---
layout: post
title:  "spring源码分析－controller的线程安全"
date:   2010-07-06 16:26:57
author: 
categories: program
---

## spring源码分析－controller的线程安全
### by 
### at 2010-07-06 16:26:57
### original <http://www.javaeye.com/topic/706741>

  大家都知道，struts1.2由于是线程安全的，每一个请求都去实例化一个action，造成大量并发时的资源浪费。
<br>  struts2在这一点上做了改进，每个action都是一个singleton，所有的请求都是请求同一个action实例。这样在一定程度上能节约资源，但又有安全问题。最常见的就是在action中声明有块状的实例变量，因为这一点是不被提倡的。如果一定要声明，那一定要加上同步块。
<br>  那么在spring mvc中的controller是不是线程安全的呢？答案是否定的。controller在默认情况下也是非线程安全的，我们来看看源码：
<br>
<br><pre name="code"> * @author John A. Lewis
 * @author Juergen Hoeller
 * @since 2.0
 * @see ResourceAwareController
 * @see EventAwareController
 */
public abstract class AbstractController extends PortletContentGenerator implements Controller {

	[color=red]private boolean synchronizeOnSession = false;[/color]</pre>
<br>
<br>由上面源码可知，controller默认是非安全的。
<br>
<br><pre name="code">	public void handleActionRequest(ActionRequest request, ActionResponse response) throws Exception {
		// Delegate to PortletContentGenerator for checking and preparing.
		check(request, response);

		// Execute in synchronized block if required.
                //只有synchronizeOnSession设置为true，才会同步处理请求
		if (this.synchronizeOnSession) {
			PortletSession session = request.getPortletSession(false);
			if (session != null) {
				synchronized (session) {
					handleActionRequestInternal(request, response);
					return;
				}
			}
		}

		handleActionRequestInternal(request, response);
	}</pre>
<br>
<br>只有手工设置controller的synchronizeOnSession值为true，才会被同步处理。
<br>
<br>因此，我们在使用spring mvc 的contrller时，应避免在controller中定义实例变量。
          
          <br><br>
          作者: <a href="http://jakoes.javaeye.com">jakoes</a> 
          <br>
          声明: 本文系JavaEye网站发布的原创文章，未经作者书面许可，严禁任何网站转载本文，否则必将追究法律责任！
          <br><br>
          <span style="color:red">
            <a href="http://www.javaeye.com/topic/706741" style="color:red">已有 <strong>4</strong> 人发表回复，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
          </span>
          <br><br><br>
<span style="color:#e28822">JavaEye推荐</span>
<br>
<ul><li><a href="http://www.iteye.com/clicks/138"><span style="color:red;font-weight:bold">上海：高薪诚聘Python开发人员</span></a></li><li><a href="http://www.iteye.com/clicks/269"><span style="color:red;font-weight:bold">北京：手机之家网站诚聘PHP程序员</span></a></li></ul>
<br><br><br>