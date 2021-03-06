---
layout: post
title:  "Flex与Java Servlet交互实例"
date:   2010-07-26 11:53:27
author: 
categories: program
---

## Flex与Java Servlet交互实例
### by 
### at 2010-07-26 11:53:27
### original <http://www.javaeye.com/topic/721272>

自从参加了flex培训，不知不觉，学习Flex已经二十天有余了！现在已经基本进入状态，今天本人在此班门弄斧，自己写一个用Flex与后台的Servlet进行通讯的例子，希望对一些刚学习Flex的新手有所帮助，目的就是为了让后面的兄弟少走弯路。
<br>交互原理：三个字母---------XML，客户端接受从服务器端发送过来的XML数据。
<br>本例工作流程：客户端很简单就一个DataGrid组件，用来显示服务器端传送过来的数据。对应的mxml文件如下：
<br>xml 代码
<br>1.	&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;      
<br>2.	&lt;mx:Application xmlns:mx=&quot;http://www.adobe.com/2006/mxml&quot; layout=&quot;absolute&quot;&gt;      
<br>3.	    &lt;mx:Model source=&quot;http://localhost:8080/flex/first&quot; id=&quot;model&quot;&gt;      
<br>4.	               
<br>5.	    &lt;/&lt; SPAN&gt;mx:Model&gt;      
<br>6.	    &lt;mx:Panel title=&quot;用户信息&quot; width=&quot;776&quot; height=&quot;281&quot; fontSize=&quot;18&quot;&gt;      
<br>7.	        &lt;mx:DataGrid dataProvider=&quot;{model.user}&quot; width=&quot;748&quot; height=&quot;231&quot;&gt;      
<br>8.	            &lt;mx:columns&gt;      
<br>9.	                &lt;mx:DataGridColumn dataField=&quot;name&quot; headerText=&quot;用户&quot;&gt;      
<br>10.	                           
<br>11.	                &lt;/&lt; SPAN&gt;mx:DataGridColumn&gt;      
<br>12.	                &lt;mx:DataGridColumn dataField=&quot;pwd&quot; headerText=&quot;密码&quot;&gt;      
<br>13.	                           
<br>14.	                &lt;/&lt; SPAN&gt;mx:DataGridColumn&gt;      
<br>15.	                &lt;mx:DataGridColumn dataField=&quot;school&quot; headerText=&quot;现在学校&quot;&gt;      
<br>16.	                           
<br>17.	                &lt;/&lt; SPAN&gt;mx:DataGridColumn&gt;      
<br>18.	            &lt;/&lt; SPAN&gt;mx:columns&gt;      
<br>19.	        &lt;/&lt; SPAN&gt;mx:DataGrid&gt;      
<br>20.	    &lt;/&lt; SPAN&gt;mx:Panel&gt;      
<br>21.	&lt;/&lt; SPAN&gt;mx:Application&gt;     
<br>在此需要注意标签，source属性指定的是一个servlet映射，这个servlet的作用是利用response向客户端写入XML。
<br>读取数据库数据，生成XML文件由两个类组成，一个为FirstServlet.java，一个为XML.java，其中前者就是一个普通的servlet，用来写XML文件，后者专门用来生成XML文件，由Java XML API操作完成。废话少说，先看看代码
<br>FirstServlet代码摘要：
<br>java 代码
<br>1.	try {       
<br>2.	            xml.init() ;       
<br>3.	            Class.forName(&quot;com.microsoft.jdbc.sqlserver.SQLServerDriver&quot;) ;       
<br>4.	            //*********建立数据库 名为flex*************//       
<br>5.	            String url = &quot;jdbc:microsoft:sqlserver://localhost:1433;DatabaseName=flex&quot; ;       
<br>6.	            //**********换上你自己的用户名和密码信息************//       
<br>7.	            con = DriverManager.getConnection(url, &quot;sa&quot;, &quot;135780&quot;) ;       
<br>8.	            stmt = con.createStatement() ;       
<br>9.	            //**********里面建表名为USERS  具体SQL语句见附件下载**********//       
<br>10.	            result = stmt.executeQuery(&quot;select * from USERS&quot;) ;       
<br>11.	        } catch(Exception e) {       
<br>12.	            e.printStackTrace() ;       
<br>13.	        }       
<br>14.	        //重要：设置响应格式为XML格式       
<br>15.	        response.setContentType(&quot;text/xml&quot;) ;       
<br>16.	        response.setCharacterEncoding(&quot;UTF-8&quot;) ;       
<br>17.	        PrintWriter out = response.getWriter() ;     
<br>以上为servlet连接数据库并设置响应格式的代码，下面是写XML文件的关键代码：
<br>java 代码
<br>1.	while(result.next()) {       
<br>2.	                String[] strs = new String[3] ;       
<br>3.	                strs[0] = result.getString(&quot;name&quot;) ;       
<br>4.	                strs[1] = result.getString(&quot;pwd&quot;) ;       
<br>5.	                strs[2] = result.getString(&quot;school&quot;) ;       
<br>6.	                //*****创建XMLdocument*******//       
<br>7.	                xml.create(strs) ;       
<br>8.	            }       
<br>9.	            result.close() ;       
<br>10.	            stmt.close() ;       
<br>11.	            con.close() ;       
<br>12.	            //*******写XML文件到客户端********//       
<br>13.	            xml.output(out) ;     
<br>其中末行的xml是XML.java的一个实例，XML的一些重要方法如下：
<br>java 代码
<br>1.	/**     
<br>2.	     * 把XML文档写入到输出流     
<br>3.	     *      
<br>4.	     * @param out     
<br>5.	     *       ----指定的输出流     
<br>6.	     * @throws Exception     
<br>7.	     *      
<br>8.	     *      
<br>9.	     */      
<br>10.	    public void output(Writer writer) throws Exception{       
<br>11.	        Transformer trans = TransformerFactory.newInstance().newTransformer() ;       
<br>12.	        trans.setOutputProperty(OutputKeys.ENCODING, &quot;UTF-8&quot;) ;       
<br>13.	        Source source = new DOMSource(document) ;       
<br>14.	        Result result = new StreamResult(writer) ;       
<br>15.	        trans.transform(source, result) ;       
<br>16.	        writer.flush() ;       
<br>17.	        writer.close() ;       
<br>18.	    }     
<br> 
<br>java 代码
<br>1.	/**     
<br>2.	     * 创建XML文档     
<br>3.	     *      
<br>4.	     * @param   strs      
<br>5.	     *       -------传送过来的姓名和密码和学校参数     
<br>6.	     *      
<br>7.	     *      
<br>8.	     */      
<br>9.	    public void create(String[] strs) {       
<br>10.	        //******第一级子节点******//       
<br>11.	        Element first = document.createElement(&quot;user&quot;) ;       
<br>12.	        root.appendChild(first) ;       
<br>13.	               
<br>14.	        for(int i=0; i&lt;&gt; 
<br>15.	            if(i==0) {       
<br>16.	                //*******第二级子节点******//       
<br>17.	                Element name = document.createElement(&quot;name&quot;) ;       
<br>18.	                name.appendChild(document.createTextNode(strs[i])) ;       
<br>19.	                first.appendChild(name) ;       
<br>20.	            } if(i==1) {       
<br>21.	                Element pwd = document.createElement(&quot;pwd&quot;) ;       
<br>22.	                pwd.appendChild(document.createTextNode(strs[i])) ;       
<br>23.	                first.appendChild(pwd) ;       
<br>24.	            } if(i==2) {       
<br>25.	                Element school = document.createElement(&quot;school&quot;) ;       
<br>26.	                school.appendChild(document.createTextNode(strs[i])) ;       
<br>27.	                first.appendChild(school) ;       
<br>28.	            }       
<br>29.	        }       
<br>30.	    }     
<br>其实说到底，这个例子没有什么神秘的，只要记住，XML是Flex与后台进行数据交换的媒介，无论后台数据怎么千变万化，出口就有这一个。如果试验，可以先从简单开始，不涉及查询数据库，直接在servlet写出XML。
<br>详细的代码和运行在<a href="http://www.flexedu.com/zixun_show.php?id=132&amp;xid=3">http://www.flexedu.com/zixun_show.php?id=132&amp;xid=3</a>可以查看。
<br>
          
          <br><br>
          作者: <a href="http://fengyuikaka.javaeye.com">fengyuikaka</a> 
          <br>
          声明: 本文系JavaEye网站发布的原创文章，未经作者书面许可，严禁任何网站转载本文，否则必将追究法律责任！
          <br><br>
          <span style="color:red">
            <a href="http://www.javaeye.com/topic/721272" style="color:red">已有 <strong>2</strong> 人发表回复，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
          </span>
          <br><br><br>
<span style="color:#e28822">JavaEye推荐</span>
<br>
<ul><li><a href="http://www.iteye.com/clicks/138"><span style="color:red;font-weight:bold">上海：高薪诚聘Python开发人员</span></a></li><li><a href="http://www.iteye.com/clicks/269"><span style="color:red;font-weight:bold">北京：手机之家网站诚聘PHP程序员</span></a></li></ul>
<br><br><br>