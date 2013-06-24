---
layout: post
title:  "JRails Java ORM 终极解决方案"
date:   2010-10-10 15:43:02
author: 
categories: program
---

## JRails Java ORM 终极解决方案
### by 
### at 2010-10-10 15:43:02
### original <http://www.javaeye.com/topic/780453>

特点：开发速度快，代码量少，对比于Hibernate开发速度快80%以上。
<br>
<br><span style="font-size:small">
<br>项目主页:  http://www.jrails.org 
<br>Blog: http://jack.jrails.org
<br>svn checkout http://jrails-core.googlecode.com/svn/trunk/
<br>
<br>有兴趣投身开发团队的请留言给我。
<br></span>
<br><ul>
<li>简单示例 1
</li></ul>
<br>配置全局数据库连接  src/config/db.properties
<br><pre name="code">
System.db = global_db
global_db.datasource =org.apache.commons.dbcp.BasicDataSource
global_db.driver = org.gjt.mm.mysql.Driver
global_db.url = jdbc:mysql://localhost:3306/demo
global_db.username = root
global_db.password = root

#声明数据表使用自动增长类型
#可以是Auto或者UUID
System.primaryKey.type = Auto
#为true则插入数据后自动填充主键增长值
System.primaryKey.call = true

</pre>
<br>
<br>表名通常使用下划线命名规定，如product_item
<br><pre name="code">

DROP TABLE IF EXISTS `person`;
CREATE TABLE `person` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(10) DEFAULT NULL,
  `age` int(11) DEFAULT NULL,
  `sex` char(1) DEFAULT NULL,
  `income` double DEFAULT NULL,
  `phone` varchar(20) DEFAULT NULL,
  `created_at` datetime DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8;
</pre>
<br>
<br>
<br>Person 使用帕斯卡(pascal)命名法与表 person 绑定, 继承 ActiveRecord 实现键值映射
<br><pre name="code">
package app.model;

import java.sql.SQLException;
import javax.sql.DataSource;
import java.util.Map;

import org.apache.log4j.Logger;

import org.rails.core.db.DataSourceHelper;
import org.rails.core.model.ActiveRecord;
import org.rails.core.model.ObjectNotFoundException;

public class Person extends ActiveRecord {
	
	public static DataSource gloDS;
	
	static{
		try {
			//获取db.properties设置的System.db
			gloDS = DataSourceHelper.getSystemDataSource();
		} catch (Exception e) {
			Logger.getLogger(&quot;Person&quot;).error(e.getMessage(),e);
		}
	}	

	public Person() {
		super(gloDS);
	}

	public Person(Object id)
			throws ObjectNotFoundException, SQLException {
		super(gloDS, id);
	}

	public Person(Map&lt;String, Object&gt; m) {
		super(gloDS, m);
	}

}

</pre>
<br>
<br>CRUD 示例
<br><pre name="code">
package test;

import java.sql.SQLException;
import java.sql.Timestamp;
import java.util.Date;

import org.rails.core.model.ObjectNotFoundException;
import org.rails.core.model.attribute.AttributeException;

import app.model.Person;
import junit.framework.TestCase;


public class CRUDTest extends TestCase {

	public CRUDTest() {
		super();
	}

	public CRUDTest(String name) {
		super(name);
	}
	
	/**
	 * 测试插入操作
	 * @throws SQLException 
	 * @throws AttributeException 
	 */
	public void testCreate() throws AttributeException, SQLException{
		Person person = new Person();
		person.put("name","刘备");
		person.put("age",36);
		person.put("sex","M");
		person.put("income",100000.89d);
		person.put("phone","020-13812345678");
		person.put("created_at",new Timestamp(new Date().getTime()));
		assertEquals(true,person.create());
	}
	
	/**
	 * 测试查询操作,获取主键为 1 的对象
	 * @throws AttributeException
	 * @throws SQLException
	 * @throws ObjectNotFoundException
	 */
	public void testRead() throws AttributeException, 
			SQLException, ObjectNotFoundException{
		Person person = new Person(1);
		System.out.println(person);
		assertNotNull(person);
	}
	
	
	/**
	 * 测试更新操作
	 * @throws AttributeException
	 * @throws SQLException
	 * @throws ObjectNotFoundException
	 */
	public void testUpdate() throws AttributeException, 
			SQLException, ObjectNotFoundException{
		Person person = new Person(1);
		person.put("name","刘备");
		person.put("age",38);
		person.put("income",106000.00d);
		person.put("phone","010-888888888");
		person.put("created_at",new Timestamp(new Date().getTime()));
		assertEquals(true,person.update());
	}
	
	/**
	 * 测试删除操作
	 * @throws ObjectNotFoundException
	 * @throws SQLException
	 */
	public void testDelete() throws ObjectNotFoundException, SQLException{
		Person person = new Person(1);
		
		assertEquals(true,person.delete());
	}

}

</pre>
          
  <br><br>
  <ul>
    本文附件下载:
    
      <li><a href="http://dl.javaeye.com/topics/download/d398e1ab-4c64-3dd3-aaf3-87a2ebb34988">crud_demo101010.rar</a> (1.3 MB)</li>
    
  </ul>

          <br><br>
          作者: <a href="http://jrails.javaeye.com">jrails</a> 
          <br>
          声明: 本文系JavaEye网站发布的原创文章，未经作者书面许可，严禁任何网站转载本文，否则必将追究法律责任！
          <br><br>
          <span style="color:red">
            <a href="http://www.javaeye.com/topic/780453" style="color:red">已有 <strong>0</strong> 人发表回复，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
          </span>
          <br><br><br>
<span style="color:#e28822">JavaEye推荐</span>
<br>
<ul><li><a href="http://www.iteye.com/clicks/138"><span style="color:red;font-weight:bold">上海：高薪诚聘php开发人员</span></a></li><li><a href="http://www.iteye.com/clicks/439"><span style="color:blue;font-weight:bold">限时报名参加Oracle技术大会</span></a></li><li><a href="http://www.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>