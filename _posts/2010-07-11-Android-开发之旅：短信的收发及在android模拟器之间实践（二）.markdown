---
layout: post
title:  "Android 开发之旅：短信的收发及在android模拟器之间实践（二）"
date:   2010-07-11 12:36:00
author: 吴秦
categories: program
---

## Android 开发之旅：短信的收发及在android模拟器之间实践（二）
### by 吴秦
### at 2010-07-11 12:36:00
### original <http://www.cnblogs.com/skynet/archive/2010/07/11/1775166.html>

<p><a href="http://www.cnblogs.com/skynet/"><img src="http://pic.cnblogs.com/face/u92071.jpg" alt="" border="0"></a><br>作者: <a href="http://www.cnblogs.com/skynet/">吴秦</a> 发表于 2010-07-11 12:36 <a href="http://www.cnblogs.com/skynet/archive/2010/07/11/1775166.html">原文链接</a> 阅读: 726 评论: 1</p><h4><b>引言</b></h4>
<p><a href="http://www.cnblogs.com/skynet/archive/2010/06/14/1758284.html">前面</a>我们介绍都只是如何发送SMS消息，接下来我们介绍如何接收SMS消息，及另一种发短信的方式并增强为可以发生图片等，最后介绍一下emulator工具。本文的主要内容如下：</p>
<ul>
<li>1~5见<a href="http://www.cnblogs.com/skynet/archive/2010/06/14/1758284.html">Android 开发之旅：短信的收发及在android模拟器之间实践（一）</a>  </li>
<li>6、温故知新之Intent  </li>
<li>7、准备工作：SmsMessage类  </li>
<li>8、SMS接收程序  </li>
<li>9、另一种发送短信的方式：使用Intent  </li>
<li>10、增强SMS为MMS</li>
</ul>
<h4><b>6、温故知新之Intent</b></h4>
<p>此系列前面简单地接受过意图（Intent），这里再次简单介绍一下，在短信接收程序和使用Intent发送SMS中我们要用到。android应用程序的三大组件——Activities、Services、Broadcast Receiver，通过消息触发，这个消息就称作意图（Intent）。下面以Acitvity为例，介绍一下Intent。Android用Intent这个特殊的类实现在Activity与Activity之间的切换。<b>Intent类用于描述应用的功能。在Intent的描述结构中，有两个最重要的部分：动作和动作对应的数据。</b>典型的动作类型有MAIN、VIEW、PICK、EDIT等，我们在短信接收程序中就用到从广播意图中提取动作类型并判断是否是"<span style="color:#ff8040">android.provider.Telephony.SMS_RECEIVED</span>"，进而作深一步的处理。而动作对应的数据则以URI的形式表示。例如，要查看一个人的联系方式，需要创建一个动作为VIEW的Intent，以及表示这个人的URI。</p>
<p>通过解析各种Intent，从一个屏幕导航到另一个屏幕是很简单的。当向前导航时，Activity将会调用<span style="color:#ff8040">startActivity("指定一个Intent")</span>方法。然后，系统会在所有已安装的应用程序中定义的IntentFilter中查找，找到最匹配的Intent对应的Activity。新的Activity接收到指定的Intent的通知后，开始运行。当<span style="color:#ff8040">startActivity()</span>方法被调用时，将触发解析指定Intent的动作，该机制提供了两个关键的好处：</p>
<ul>
<li>Activity能够重复利用从其他组件中以Intent形式产生的请求。  </li>
<li>Activity可以在任何时候被具有相同IntentFilter的新的Activity取代。</li>
</ul>
<h4><b>7、准备工作：SmsMessage类</b></h4>
<p>顾名思义，SmsMessage类是一个表示短信的类，为了更好地了解Android的短信机制及以后更好地编写短信相关程序，这里介绍一下该类的公有方法和常量，及嵌套枚举、类成员。</p>
<p><b>公有方法：</b></p>
<ul>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">static</span> <span style="color:#0000ff">int</span>[]  calculateLength  (CharSequence  msgBody, <span style="color:#0000ff">boolean</span> use7bitOnly)<br><i>参数</i>：<br><i>msgBody</i>-要封装的消息、<i>use7bitOnly</i>-如果为TRUE，不是广播特定7-比特编码的部分字符被认为是单个空字符；如果为FALSE，且<i>msgBody</i>包含非7-比特可编码字符，长度计算使用16-比特编码。<br><i>返回值</i>：<br>返回一个4个元素的int数组，int[0]表示要求使用的SMS数量、int[1]表示编码单元已使用的数量、int[2]表示剩余到下个消息的编码单元数量、int[3]表示编码单元大小的指示器。  </li>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">static</span> <span style="color:#0000ff">int</span>[]  calculateLength  (String  messageBody, <span style="color:#0000ff">boolean</span> use7bitOnly)<br><i>参数</i>和<i>返回值</i>跟上面类似  </li>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">static</span> SmsMessage   createFromPdu  (<span style="color:#0000ff">byte</span>[] pdu)<br>从原始的PDU（protocol description units）创建一个SmsMessage。这个方法很重要，在我们编写短信接收程序要用到，它从我们接收到的广播意图中获取的字节创建SmsMessage。  </li>
<li><span style="color:#0000ff">public</span> String  getDisplayMessageBody()<br>返回短信消息的主体，或者Email消息主体（如果这个消息来自一个Email网关）。如果消息主体不可用，返回null。这个方法也很重要，在我们编写短信接收程序也要用到。  </li>
<li><span style="color:#0000ff">public</span> String   getDisplayOriginatingAddress  ()<br>返回信息来源地址，或Email地址（如果消息来自Email网关）。如果消息主体不可用，返回null。这个方法在来电显示，短信接收程序中经常用到。  </li>
<li><span style="color:#0000ff">public</span> String   getEmailBody  ()<br>如果isEmail为TRUE，即是邮件，返回通过网关发送Email的地址，否则返回null。  </li>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">int</span>  getIndexOnIcc  ()<br>返回消息记录在ICC上的索引（从1开始的）  </li>
<li><span style="color:#0000ff">public</span> String   getMessageBody  ()<br>以一个String返回消息的主体，如果它存在且是基于文本的。  </li>
<li><span style="color:#0000ff">public</span> SmsMessage.MessageClass   getMessageClass  ()<br>返回消息的类。  </li>
<li><span style="color:#0000ff">public</span> String   getOriginatingAddress  ()<br>以String返回SMS信息的来电地址，或不可用时为null。  </li>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">byte</span>[]  getPdu  ()<br>返回消息的原始PDU数据。  </li>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">int</span>  getProtocolIdentifier  ()<br>获取协议标识符。  </li>
<li><span style="color:#0000ff">public</span> String   getPseudoSubject  ()  </li>
<li><span style="color:#0000ff">public</span> String   getServiceCenterAddress  ()<br>返回转播消息SMS服务中心的地址，如果没有的话为null。  </li>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">int</span>  getStatus  ()<br>GSM：为一个SMS-STATUS-REPORT消息，它返回状态报告的status字段。这个字段表示之前提交的SMS消息的状态。<br>CDMA：为不影响来自GSM的状态码，值移动到31-16比特。这个值由一个error类（25-16比特）和一个状态码（23-16比特）组成。<br>如果是0，表示之前发送的消息已经被收到。  </li>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">int</span>  getStatusOnIcc  ()<br>返回消息在ICC上的状态（已读、未读、已发送、未发送）。有下面的几个值：SmsManager.STATUS_ON_ICC_FREE、SmsManager.STATUS_ON_ICC_READ、SmsManager.STATUS_ON_ICC_UNREAD、SmsManager.STATUS_ON_ICC_SEND、SmsManager.STATUS_ON_ICC_UNSENT这几个值在<a href="http://www.cnblogs.com/skynet/archive/2010/06/14/1758284.html">上篇</a>的SmsManager类介绍有讲到。  </li>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">static</span> SmsMessage.SubmitPdu   getSubmitPdu  (<br>       String  scAddress, String  destinationAddress, <br>       <span style="color:#0000ff">short</span> destinationPort, <span style="color:#0000ff">byte</span>[] data, <br>       <span style="color:#0000ff">boolean</span> statusReportRequested)<br><i>参数</i>：<i>scAddress </i>- 服务中心的地址（Sercvice Centre address，为null即使用默认的）、<i>destinationAddress</i> - 消息的目的地址、<i>destinationPort</i>- 发送消息到目的的端口号、<i>data</i> - 消息数据。<br><i>返回值</i>：一个包含编码了的SC地址（如果指定了的话）和消息内容的SubmitPdu，否则返回null，如果编码错误。  </li>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">static</span> SmsMessage.SubmitPdu   getSubmitPdu  (<br>       String  scAddress, String  destinationAddress,<br>       String  message, <span style="color:#0000ff">boolean</span> statusReportRequested)<br>和上面类似。  </li>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">static</span> <span style="color:#0000ff">int</span>  getTPLayerLengthForPDU  (String  pdu)<br>返回指定SMS-SUBMIT PDU的TP-Layer-Length，长度单位是字节而不是十六进字符。  </li>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">long</span>  getTimestampMillis  ()<br>以currentTimeMillis()格式返回服务中心时间戳。  </li>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">byte</span>[]  getUserData  ()<br>返回用户数据减去用户数据头部（如果有的话）  </li>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">boolean</span>  isCphsMwiMessage  ()<br>判断是否是CPHS MWI消息  </li>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">boolean</span>  isEmail  ()<br>判断是否是Email，如果消息来自一个Email网关且Email发送者（sender）、主题（subject）、解析主体（parsed body）可用，则返回TRUE。  </li>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">boolean</span>  isMWIClearMessage  ()<br>判断消息是否是一个CPHS 语音邮件或消息等待MWI清除（clear）消息。  </li>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">boolean</span>  isMWISetMessage  ()<br>判断消息是否是一个CPHS 语音邮件或消息等待MWI设置（set）消息。  </li>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">boolean</span>  isMwiDontStore  ()<br>如果消息是一个“Message Waiting Indication Group：Discard Message”通知且不应该保存，则返回TRUE，否则返回FALSE。  </li>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">boolean</span>  isReplace  ()<br>判断是否是一个“replace short message”SMS  </li>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">boolean</span>  isReplyPathPresent  ()<br>判断消息的TP-Reply-Path位是否在消息中设置了。  </li>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">boolean</span>  isStatusReportMessage  ()<br>判断是否是一个SMS-STATUS-REPORT消息。</li>
</ul>
<p><b>常量值：</b></p>
<ul>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">static</span> <span style="color:#0000ff">final</span> <span style="color:#0000ff">int</span>  ENCODING_16BIT ：值为3(0x00000003)  </li>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">static</span> <span style="color:#0000ff">final</span> <span style="color:#0000ff">int</span>  ENCODING_8BIT ：值为2 (0x00000002)  </li>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">static</span> <span style="color:#0000ff">final</span> <span style="color:#0000ff">int</span>  ENCODING_UNKNOWN ：值为0 (0x00000000) ，用户数据编码单元的大小。  </li>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">static</span> <span style="color:#0000ff">final</span> <span style="color:#0000ff">int</span>  MAX_USER_DATA_BYTES ：值为140 (0x0000008c)，表示每个消息的最大负载字节数。  </li>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">static</span> <span style="color:#0000ff">final</span> <span style="color:#0000ff">int</span>  MAX_USER_DATA_BYTES_WITH_HEADER ：134 (0x00000086)，如果一个用户数据有头部，该值表示它的最大负载字节数，该值假定头部仅包含CONCATENATED_8_BIT_REFENENCE元素。  </li>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">static</span> <span style="color:#0000ff">final</span> <span style="color:#0000ff">int</span>  MAX_USER_DATA_SEPTETS ：值为160 (0x000000a0) ，表示每个消息的最大负载septets数。  </li>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">static</span> <span style="color:#0000ff">final</span> <span style="color:#0000ff">int</span>  MAX_USER_DATA_SEPTETS_WITH_HEADER ：值为153 (0x00000099)，如果存在用户数据头部，则该值表示最大负载septets数该值假定头部仅包含CONCATENATED_8_BIT_REFENENCE元素。</li>
</ul>
<p><b>嵌套枚举成员SmsMessage.MessageClass的枚举值：</b></p>
<ul>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">static</span> <span style="color:#0000ff">final</span> SmsMessage.MessageClass   CLASS_0  </li>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">static</span> <span style="color:#0000ff">final</span> SmsMessage.MessageClass   CLASS_1  </li>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">static</span> <span style="color:#0000ff">final</span> SmsMessage.MessageClass   CLASS_2  </li>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">static</span> <span style="color:#0000ff">final</span> SmsMessage.MessageClass   CLASS_3  </li>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">static</span> <span style="color:#0000ff">final</span> SmsMessage.MessageClass   CLASS_UNKNOWN</li>
</ul>
<p><b>嵌套枚举成员SmsMessage.MessageClass的公有方法：</b></p>
<ul>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">static</span> SmsMessage.MessageClass valueOf (String name)：返回值的字符串的值  </li>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">static</span> <span style="color:#0000ff">final</span> MessageClass[]   values  ()：返回MessageClass的值数组</li>
</ul>
<p><b>嵌套类成员SmsMessage.SubmitPdu的字段：</b></p>
<ul>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">byte</span>[]  encodedMessage ：编码了的消息  </li>
<li><span style="color:#0000ff">public</span> <span style="color:#0000ff">byte</span>[]  encodedScAddress ：编码的服务中心地址</li>
</ul>
<p><b>嵌套类成员SmsMessage.SubmitPdu的公有方法：</b></p>
<ul>
<li><span style="color:#0000ff">public</span> String   toString  ()<br>返回一个包含简单的、可读的这个对象的描述字符串。鼓励子类去重写这个方法，并提供实现对象的类型和数据。默认实现简单地连接类名、@、十六进制表示的对象哈希码，即下面的形式： getClass().getName() + '@' + Integer.toHexString(hashCode())</li>
</ul>
<h4><b>8、SMS接收程序</b></h4>
<p>当一个SMS消息被接收时，一个新的广播意图由android.provider.Telepony.SMS_RECEIVED动作触发。注意：这个一个字符串字面量（string  literal），但是SDK当前并没有包括这个字符串的引用，因此当要在应用程序中使用它时必须自己显示的指定它。现在我们就开始构建一个SMS接收程序：</p>
<p>1)、跟SMS发送程序类似，要在清单文件<span style="color:#ff8040">AndroidManifest.xml</span>中指定权限允许接收SMS：<span style="color:#0000ff">&lt;</span><span style="color:#800000">uses</span>-<span style="color:#ff0000">permission</span> <span style="color:#ff0000">android</span>:<span style="color:#ff0000">name</span>=<span style="color:#0000ff">"android.permission.RECEIVER_SMS"</span><span style="color:#0000ff">/&gt;</span></p>
<p>为了能够回发短信，还应该加上发送的权限。</p>
<p>2)、应用程序监听SMS意图广播，SMS广播意图包含了到来的SMS细节。我们要从其中提取出<span style="color:#ff8040">SmsMessage</span>对象，这样就要用到<span style="color:#ff8040">pdu</span>键提取一个SMS PDUs数组（protocol description units—封装了一个SMS消息和它的元数据），每个元素表示一个SMS消息。为了将每个PDU byte数组转化为一个SMS消息对象，需要调用<span style="color:#ff8040">SmsMessage.createFromPdu</span>。</p>
<p>每个<span style="color:#ff8040">SmsMessage</span>包含SMS消息的详细信息，包括起始地址（电话号码）、时间戳、消息体。下面编写一个接收短信的类SmsReceiver代码如下：</p>
<p><span>
<div>
<pre><img src="http://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif" width="11" align="top" height="16"><img src="http://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif" width="11" align="top" height="16"><span>SmsReceiver类的完整代码</span><span><span style="color:#0000ff">package</span> skynet.com.conblogs.www;

<span style="color:#0000ff">import</span> android.content.BroadcastReceiver;
<span style="color:#0000ff">import</span> android.content.Context;
<span style="color:#0000ff">import</span> android.content.Intent;
<span style="color:#0000ff">import</span> android.os.Bundle;
<span style="color:#0000ff">import</span> android.telephony.SmsManager;
<span style="color:#0000ff">import</span> android.telephony.SmsMessage;
<span style="color:#0000ff">import</span> android.widget.Toast;

<span style="color:#0000ff">public</span> <span style="color:#0000ff">class</span> SmsReceiver <span style="color:#0000ff">extends</span> BroadcastReceiver {
	@Override
	<span style="color:#0000ff">public</span> <span style="color:#0000ff">void</span> onReceive(Context _context, Intent _intent) {
		<span style="color:#0000ff">if</span> (_intent.getAction().equals(SMS_RECEIVER)) {
			SmsManager sms = SmsManager.getDefault();

			Bundle bundle = _intent.getExtras();
			<span style="color:#0000ff">if</span> (bundle != <span style="color:#0000ff">null</span>) {
				Object[] pdus = (Object[]) bundle.get("<span style="color:#8b0000">pdus</span>");
				SmsMessage[] messages = <span style="color:#0000ff">new</span> SmsMessage[pdus.length];
				<span style="color:#0000ff">for</span> (<span style="color:#0000ff">int</span> i = 0; i &lt; pdus.length; i++)
					messages[i] = SmsMessage.createFromPdu((<span style="color:#0000ff">byte</span>[]) pdus[i]);
				<span style="color:#0000ff">for</span> (SmsMessage message : messages) {
					String msg = message.getMessageBody();
					String to = message.getOriginatingAddress();
					<span style="color:#0000ff">if</span> (msg.toLowerCase().startsWith(queryString)) {
						String out = msg.substring(queryString.length());
						sms.sendTextMessage(to, <span style="color:#0000ff">null</span>, out, <span style="color:#0000ff">null</span>, <span style="color:#0000ff">null</span>);

						Toast.makeText(_context, "<span style="color:#8b0000">success</span>", 
								Toast.LENGTH_LONG).show();
					}
				}
			}
		}
	}
    
	<span style="color:#0000ff">private</span> <span style="color:#0000ff">static</span> <span style="color:#0000ff">final</span> String queryString="<span style="color:#8b0000">@echo</span>";
	<span style="color:#0000ff">private</span> <span style="color:#0000ff">static</span> <span style="color:#0000ff">final</span> String SMS_RECEIVER=
		"<span style="color:#8b0000">android.provider.Telephony.SMS_RECEIVED</span>";
}</span></pre>
</div>
<br></span></p>
<p>上面代码的功能是从接收到的广播意图中提取来电号码、短信内容，然后将短信加上@echo头部回发给来电号码，并在屏幕上显示一个Toast消息提示成功。</p>
<h4><b>9、另一种发送短信的方式：使用Intent</b></h4>
<p><a href="http://www.cnblogs.com/skynet/archive/2010/06/14/1758284.html">上篇</a>我们使用SmsManager类实现了发送SMS的功能，且并没有用到内置的客户端。实际上，我们很少这样做，自己在应用程序中去完全实现一个完整的SMS客户端。相反我们会去利用它，将需要发送的内容和目的手机号传递给内置的SMS客户端，然后发送。</p>
<p>下面我就向大家介绍如何利用Intent实现利用将我们的东西传递给内置SMS客户端发送我们SMS。为了实现这个功能，就要用到<span style="color:#ff8040">startActivity("指定一个Intent")</span>方法，且指定Intent的动作为<span style="color:#ff8040">Intent.ACTION_SENDTO</span>，用<span style="color:#ff8040">sms：</span>指定目标手机号，用<span style="color:#ff8040">sms_body</span>指定信息内容。java源文件如下所示：</p>
<div>
<pre><img style="display:inline" src="http://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif" width="11" align="top" height="16"><img src="http://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif" width="11" align="top" height="16"><span style="display:inline">java源文件完整代码</span><span><span style="color:#0000ff">package</span> skynet.com.cnblogs.www;

<span style="color:#0000ff">import</span> android.app.Activity;
<span style="color:#0000ff">import</span> android.content.Intent;
<span style="color:#0000ff">import</span> android.net.Uri;
<span style="color:#0000ff">import</span> android.os.Bundle;
<span style="color:#0000ff">import</span> android.view.View;
<span style="color:#0000ff">import</span> android.widget.Button;
<span style="color:#0000ff">import</span> android.widget.EditText;
<span style="color:#0000ff">import</span> android.widget.Toast;


<span style="color:#0000ff">public</span> <span style="color:#0000ff">class</span> TextMessage <span style="color:#0000ff">extends</span> Activity {
	<span style="color:#008000">/** Called when the activity is first created. */</span>
	@Override
	<span style="color:#0000ff">public</span> <span style="color:#0000ff">void</span> onCreate(Bundle savedInstanceState) {
		<span style="color:#0000ff">super</span>.onCreate(savedInstanceState);

		setContentView(R.layout.main);
		btnSend = (Button) findViewById(R.id.btnSend);
		edtPhoneNo = (EditText) findViewById(R.id.edtPhoneNo);
		edtContent = (EditText) findViewById(R.id.edtContent);

		btnSend.setOnClickListener(<span style="color:#0000ff">new</span> View.OnClickListener() {
			<span style="color:#0000ff">public</span> <span style="color:#0000ff">void</span> onClick(View v) {
				String phoneNo = edtPhoneNo.getText().toString();
				String message = edtContent.getText().toString();
				<span style="color:#0000ff">if</span> (phoneNo.length() &gt; 0 &amp;&amp; message.length() &gt; 0) {
					 <b><span style="color:#ff0000">Intent smsIntent=<span style="color:#0000ff">new</span> Intent(Intent.ACTION_SENDTO,
							 Uri.parse("<span style="color:#8b0000">sms:</span>"+edtPhoneNo.getText().toString()));
					 smsIntent.putExtra("<span style="color:#8b0000">sms_body</span>", edtContent.getText().toString());
					 TextMessage.<span style="color:#0000ff">this</span>.startActivity(smsIntent);</span></b>
				} <span style="color:#0000ff">else</span>
					Toast.makeText(getBaseContext(),
							"<span style="color:#8b0000">Please enter both phone number and message.</span>",
							Toast.LENGTH_SHORT).show();
			}
		});
	}

	<span style="color:#0000ff">private</span> Button btnSend;
	<span style="color:#0000ff">private</span> EditText edtPhoneNo;
	<span style="color:#0000ff">private</span> EditText edtContent;
}</span></pre>
</div>
<p> </p>
<p>注意代码中的红色粗体部分，就是实现这个功能的核心代码！布局文件maim.xml和值文件string.xml跟上篇中的一样，这里不再累述。运行结果如下图：</p>
<p><a href="http://images.cnblogs.com/cnblogs_com/skynet/WindowsLiveWriter/768129bbcf7f_E171/image_2.png"><img style="display:block;float:none;margin-left:auto;margin-right:auto;border-width:0px" title="image" alt="image" src="http://images.cnblogs.com/cnblogs_com/skynet/WindowsLiveWriter/768129bbcf7f_E171/image_thumb.png" width="361" border="0" height="327"></a> </p>
<p align="center">图2、程序主界面</p>
<p>点击send按钮之后，转到内置的SMS客户端并且将我们输入的值传入了，如下图：</p>
<p><a href="http://images.cnblogs.com/cnblogs_com/skynet/WindowsLiveWriter/768129bbcf7f_E171/image_4.png"><img style="display:block;float:none;margin-left:auto;margin-right:auto;border-width:0px" title="image" alt="image" src="http://images.cnblogs.com/cnblogs_com/skynet/WindowsLiveWriter/768129bbcf7f_E171/image_thumb_1.png" width="358" border="0" height="518"></a> </p>
<p align="center">图3、内容传至内置SMS客户端</p>
<p align="left">发送之后，5556号android模拟器会收到我们发送的消息，如下图：</p>
<p align="left"><a href="http://images.cnblogs.com/cnblogs_com/skynet/WindowsLiveWriter/768129bbcf7f_E171/image_6.png"><img style="display:block;float:none;margin-left:auto;margin-right:auto;border-width:0px" title="image" alt="image" src="http://images.cnblogs.com/cnblogs_com/skynet/WindowsLiveWriter/768129bbcf7f_E171/image_thumb_2.png" width="513" border="0" height="384"></a> </p>
<p align="center">图5、发送之后5556号android模拟器收到消息</p>
<h4><b>10、增强SMS为MMS</b></h4>
<p>我们讲了这么多，都还只是实现了简单的发生SMS的功能，如果我们想发送图片、音频怎么办(⊙o⊙)？不急，现在我们就将第9节介绍的SMS发送程序改造为MMS。</p>
<p>我们可以附加一个文件到我们的消息做为附件发送，用<span style="color:#ff8040">Intent.EXTRA_STREAM</span>和附件资源的Uri做为参数调用<span style="color:#ff8040">putExtra()</span>方法，附加到信息。并设置Intent的类型为<span style="color:#ff8040">mime-type</span>。<b>要注意的是：内置的MMS并不包括一个<span style="color:#ff8040">ACTION_SENDTO</span>动作的Intent接收器，我们需要使用的动作类型是<span style="color:#ff8040">ACTION_SEND</span>，并且目标手机号不在是使用<span style="color:#ff8040">sms：</span>而是<span style="color:#ff8040">address</span>。</b>主要代码如下：</p>
<div>
<pre><b><span style="color:#008000">// Get the URI of a piece of media to attach.</span><br>Uri attached_Uri = Uri.parse("<span style="color:#8b0000">content://media/external/images/media/1</span>");<br><span style="color:#008000">// Create a new MMS intent</span><br>Intent mmsIntent = <span style="color:#0000ff">new</span> Intent(Intent.ACTION_SEND, attached_Uri);<br>mmsIntent.putExtra("<span style="color:#8b0000">sms_body</span>", edtContent.getText().toString());<br>mmsIntent.putExtra("<span style="color:#8b0000">address</span>", edtPhoneNo.getText().toString());<br>mmsIntent.putExtra(Intent.EXTRA_STREAM, attached_Uri);<br>mmsIntent.setType("<span style="color:#8b0000">image/png</span>");<br>startActivity(mmsIntent);</b><br></pre>
</div>
<p> </p>
<p>将这段代码替换第9节中的红色粗体代码，就完成而来一个MMS的构建。</p>
<p>PS：这篇文章本应该很早就该发出来了，在6月20号就写好了，但由于现在的工作环境完全隔离了Internet，家里又还没有开通网络；还有一个原因是现在工作比较忙！请大家见谅，能够继续支持我，让我有动力写下去。还有一点，这篇文章在我电脑上放久了，对当时的状态有些忘了，不知道文中有什么遗漏和错误，请大家指出！</p>
<p> </p>
<p>本系列的其它文章： 
</p>
<ul>
<li><a href="http://www.cnblogs.com/skynet/archive/2010/04/12/1709892.html">Android 开发之旅：环境搭建及HelloWorld</a>
</li>
<li><a href="http://www.cnblogs.com/skynet/archive/2010/04/13/1711479.html">Android 开发之旅：HelloWorld项目的目录结构</a>
</li>
<li><a href="http://www.cnblogs.com/skynet/archive/2010/04/15/1712924.html">Android 开发之旅：android架构</a>
</li>
<li><a href="http://www.cnblogs.com/skynet/archive/2010/04/16/1713740.html">Android 开发之旅：应用程序基础及组件</a>
</li>
<li><a href="http://www.cnblogs.com/skynet/archive/2010/04/17/1714022.html">Android 开发之旅：应用程序基础及组件（续）</a>
</li>
<li><a href="http://www.cnblogs.com/skynet/archive/2010/04/19/1715320.html">Android 开发之旅：活动与任务</a>
</li>
<li><a href="http://www.cnblogs.com/skynet/archive/2010/05/02/1726008.html">Android 开发之旅：进程与线程</a>
</li>
<li><a href="http://www.cnblogs.com/skynet/archive/2010/05/05/1727645.html">Android 开发之旅：组件生命周期（一）</a>
</li>
<li><a href="http://www.cnblogs.com/skynet/archive/2010/05/06/1729332.html">Android 开发之旅：组件生命周期（二）</a>
</li>
<li><a href="http://www.cnblogs.com/skynet/archive/2010/05/07/1730010.html">Android 开发之旅：组件生命周期（三）</a>
</li>
<li><a href="http://www.cnblogs.com/skynet/archive/2010/05/11/1732208.html">Android 开发之旅：又见Hello World！</a>
</li>
<li><a href="http://www.cnblogs.com/skynet/archive/2010/05/20/1740277.html">Android 开发之旅：深入分析布局文件&amp;又是“Hello World！”</a>
</li>
<li><a href="http://www.cnblogs.com/skynet/archive/2010/06/06/1752616.html">Android 开发之旅：view的几种布局方式及实践</a>
</li>
<li><a href="http://www.cnblogs.com/skynet/archive/2010/06/14/1758284.html">Android 开发之旅：短信的收发及在android模拟器之间实践（一）</a>
</li>
<li><a href="http://www.cnblogs.com/skynet/archive/2010/07/11/1775166.html">Android 开发之旅：短信的收发及在android模拟器之间实践（二）</a></li>
</ul><img src="http://www.cnblogs.com/skynet/aggbug/1775166.html?type=1" width="1" height="1" alt=""><p>评论: 1　<a href="http://www.cnblogs.com/skynet/archive/2010/07/11/1775166.html#pagedcomment">查看评论</a>　<a href="http://www.cnblogs.com/skynet/archive/2010/07/11/1775166.html#commentform">发表评论</a></p><p><a href="http://job.cnblogs.com/enterprise/2453/">百度期待您的加盟</a></p><hr><p>最新新闻：<br>· <a href="http://news.cnblogs.com/n/68060/">Ubuntu 10.10采用全新安装程序 界面截图</a><span style="color:gray">(2010-07-12 10:30)</span><br>· <a href="http://news.cnblogs.com/n/68058/">英国官员建议政府使用Linux取代微软产品</a><span style="color:gray">(2010-07-12 10:27)</span><br>· <a href="http://news.cnblogs.com/n/68057/">Google 定义类搜索结果格式更新</a><span style="color:gray">(2010-07-12 10:23)</span><br>· <a href="http://news.cnblogs.com/n/68056/">10 个你不一定知道的 Google 趣闻</a><span style="color:gray">(2010-07-12 10:22)</span><br>· <a href="http://news.cnblogs.com/n/68054/">移动赢家难通吃</a><span style="color:gray">(2010-07-12 10:09)</span><br></p><p>编辑推荐：<a href="http://www.cnblogs.com/teddyma/archive/2010/07/11/1775394.html">TDD in HTML &amp; JavaScript 之可行性和最佳实践</a><br></p><p>网站导航：<a href="http://www.cnblogs.com">博客园首页</a>  <a href="http://home.cnblogs.com/">个人主页</a>  <a href="http://news.cnblogs.com">新闻</a>  <a href="http://home.cnblogs.com/ing/">闪存</a>  <a href="http://home.cnblogs.com/group/">小组</a>  <a href="http://space.cnblogs.com/q/">博问</a>  <a href="http://space.cnblogs.com">社区</a>  <a href="http://kb.cnblogs.com">知识库</a></p>