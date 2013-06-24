---
layout: post
title:  "struts2 与 jfreechart的整合"
date:   2010-07-26 09:00:43
author: 
categories: program
---

## struts2 与 jfreechart的整合
### by 
### at 2010-07-26 09:00:43
### original <http://www.javaeye.com/topic/721078>

<p>显示效果：<br><img src="http://dl.javaeye.com/upload/attachment/283135/45ca4328-6c19-32d6-923f-c270fbcaeb1a.jpg" alt=""><br> 先引入相关的jar包:</p>
<p>    jcommon-1.0.12.jar     jfreechart-1.0.9.jar     struts2-jfreechart-plugin-2.1.6.jar</p>
<pre name="code">package com.example.struts.action;
import jfreeChart.JfreeChartTest;

import org.jfree.chart.JFreeChart;

import com.opensymphony.xwork2.ActionSupport;

@SuppressWarnings("serial")
public class JfreeCharAction extends ActionSupport {

	/**
	 * 定义JFreeChart对象 大家请注意在这里JFreeChart对象名只能为chart 
	 * 不能是别的 
	 * 关于这点
	 * 大家可以上struts2网站上面查看一下
	 * 
	 * http://struts.apache.org/2.x/docs/jfreechart-plugin.html
	 */
	private JFreeChart chart;

	public JFreeChart getChart() {
		return chart;
	}

	public void setChart(JFreeChart chart) {
		this.chart = chart;
	}

	@Override
	public String execute() throws Exception {
		// 调用方法
		this.chart = JfreeChartTest.createChart();
		return SUCCESS;
	}
}</pre>
<p> </p>
<pre name="code">package jfreeChart;

import java.awt.Font;
import java.io.IOException;

import org.jfree.chart.ChartFactory;
import org.jfree.chart.JFreeChart;
import org.jfree.chart.plot.PiePlot;
import org.jfree.data.general.DefaultPieDataset;

public class JfreeChartTest {

	public static JFreeChart createChart() throws IOException {
		// 数据集
		DefaultPieDataset dpd = new DefaultPieDataset();
		dpd.setValue("管理人员", 25);
		dpd.setValue("市场人员", 25);
		dpd.setValue("开发人员", 45);
		dpd.setValue("其它人员", 10);
		// 创建PieChart对象
		JFreeChart chart = ChartFactory.createPieChart3D("某公司人员组织结构图", dpd,
				true, true, false);
		utils.setFont(chart);
		return chart;
	}
}

/**
 * 设置字体
 * 
 * @author zyong
 * 
 */
class utils {
	public static void setFont(JFreeChart chart) {
		Font font = new Font("宋体", Font.ITALIC, 12);
		PiePlot plot = (PiePlot) chart.getPlot();
		chart.getTitle().setFont(font);
		plot.setLabelFont(font);
		chart.getLegend().setItemFont(font);
	}
}</pre>
<p> </p>
<pre name="code">&lt;struts&gt;
	&lt;!-- 
		关于extends继承jfreechart-default这点请大家注意
		因为在struts-default这个包里并没有result-type为chart的
		chart 定义在前面我们导入的struts2-jfreechart-plugin-2.1.6.jar
		下面的struts-plugin.xml文件中
	--&gt;
	&lt;package name=&quot;jfreechart&quot; extends=&quot;jfreechart-default&quot;&gt;
		&lt;action name=&quot;jfreechart&quot; class=&quot;com.example.struts.action.JfreeCharAction&quot;&gt;
			&lt;result name=&quot;success&quot; type=&quot;chart&quot;&gt;
				&lt;param name=&quot;width&quot;&gt;600&lt;/param&gt;
				&lt;param name=&quot;height&quot;&gt;400&lt;/param&gt;
			&lt;/result&gt;
		&lt;/action&gt;
	&lt;/package&gt;
&lt;/struts&gt;</pre>
<p> </p>
          
  <br><br>
  <ul>
    本文附件下载:
    
      <li><a href="http://dl.javaeye.com/topics/download/90a7738a-0f47-3112-98d7-76949028d10d">Struts2_29_JFreeChart_.rar</a> (9.4 KB)</li>
    
  </ul>

          <br><br>
          作者: <a href="http://johnson2132.javaeye.com">johnson2132</a> 
          <br>
          声明: 本文系JavaEye网站发布的原创文章，未经作者书面许可，严禁任何网站转载本文，否则必将追究法律责任！
          <br><br>
          <span style="color:red">
            <a href="http://www.javaeye.com/topic/721078" style="color:red">已有 <strong>5</strong> 人发表回复，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
          </span>
          <br><br><br>
<span style="color:#e28822">JavaEye推荐</span>
<br>
<ul><li><a href="http://www.iteye.com/clicks/269"><span style="color:red;font-weight:bold">北京：手机之家网站诚聘PHP程序员</span></a></li><li><a href="http://www.iteye.com/clicks/138"><span style="color:red;font-weight:bold">上海：高薪诚聘Python开发人员</span></a></li></ul>
<br><br><br>