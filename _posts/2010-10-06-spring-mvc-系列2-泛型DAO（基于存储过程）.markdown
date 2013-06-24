---
layout: post
title:  "spring mvc 系列2 泛型DAO（基于存储过程）"
date:   2010-10-06 14:05:46
author: 
categories: program
---

## spring mvc 系列2 泛型DAO（基于存储过程）
### by 
### at 2010-10-06 14:05:46
### original <http://www.javaeye.com/topic/777732>

spring mvc 系列1 中：
<br>感谢 ricoyu 提示 
<br><div>引用</div><div>
<br>1 楼 ricoyu 2010-10-02   引用 
<br>DAO层不要用◎Service，用◎Repository（没拼错的话）
<br></div>
<br>基于事务管理，由于小弟一时大意忘了加上去，感谢  icanfly  提醒。
<br>此版本已经修正以上BUG。
<br>
<br>泛型需要JDK1.5以上，因此此版本需要运行在JDK1.5以上。
<br>先看代码：
<br><pre name="code">
@Repository
public class BaseDao {
	// 存储过程前缀
	protected static String PROCEDURE_NAME_PREFIX = &quot;st_&quot;;
	
	/**
	 * 存储过程对应的表名，为空时，从操作对象类名中获取
	 * 存储过程命名方式：前缀_表名_后缀
	 * 操作对象如：ProductModel, ProductSellTableModel
	 * 对应获取表名为：productTable, ProductSellTable
	 */
	protected static String TABLE_NAME = null; 
	
	// 存储过程后缀
	protected static String PROCEDURE_NAME_ADD_SUFFIX = &quot;Add&quot;;
	protected static String PROCEDURE_NAME_UPDATE_SUFFIX = &quot;Update&quot;;
	protected static String PROCEDURE_NAME_DELETE_SUFFIX = &quot;Delete&quot;;
	protected static String PROCEDURE_NAME_QUERY_BY_PRIMARY_KEY_SUFFIX = &quot;QueryByPrimaryKey&quot;;
	
	public BaseDao(){}
	
	@Autowired
	private SimpleJdbcTemplate simpleJdbcTemplate ;
	
	//@Autowired是Spring提供的一种注入Bean的方法
	@Autowired
	private JdbcTemplate jdbcTemplate ;
	
	public SimpleJdbcTemplate getSimpleJdbcTemplate() {
		return simpleJdbcTemplate;
	}

	public void setSimpleJdbcTemplate(SimpleJdbcTemplate simpleJdbcTemplate) {
		this.simpleJdbcTemplate = simpleJdbcTemplate;
	}

	public SimpleJdbcCall createSimpleJdbcCall() {
		return new SimpleJdbcCall(this.jdbcTemplate);
	}

	public JdbcTemplate getJdbcTemplate() {
		return jdbcTemplate;
	}

	public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
		this.jdbcTemplate = jdbcTemplate;
	}
	
	/**
	 * 
	 * @功能模块: executeProcedure
	 * @方法说明: 执行存储过程 将对象t转换成存储过程参数，并执行存储过程，执行存储过程后返回此对象操作后的信息
	 * @version: 1.0
	 * @param &lt;T&gt; 泛类型
	 * @param t 要操作的对象信息
	 * @param returnClass 返回的类型，如果为null 返回类型与查询类型一致
	 * @param procedureName 存储过程名称
	 * @return Map&lt;&quot;ReturningResultSet&quot;, Object&gt;
	 * @throws
	 */
	@SuppressWarnings(&quot;unchecked&quot;)
	protected &lt;T&gt; Map executeProcedure(Class returnClass, T t, String procedureName){
		SimpleJdbcCall sjc = this.createSimpleJdbcCall();
		sjc.getJdbcTemplate().setResultsMapCaseInsensitive(true);
		sjc.withProcedureName(procedureName).returningResultSet(&quot;ReturningResultSet&quot;, BeanPropertyRowMapper.newInstance(returnClass==null?t.getClass():returnClass));
		
		SqlParameterSource param = new BeanPropertySqlParameterSource(t);
		return sjc.execute(param);
	}
	
	/**
	 * 
	 * @功能模块: querySelf
	 * @方法说明: 执行存储过程 将对象t转换成存储过程参数，并执行存储过程，执行存储过程后返回此对象操作后的信息
	 * @version: 1.0
	 * @param &lt;T&gt; 泛类型
	 * @param t 要操作的对象信息
	 * @param procedureName 存储过程名称
	 * @return T 执行存储过程后返回的对象,与传入对象类型一致
	 * @throws
	 */
	@SuppressWarnings(&quot;unchecked&quot;)
	protected &lt;T&gt; T querySelf(T t, String procedureName){
		Map returnResultSet = executeProcedure(null, t, procedureName);
		List list = (List)returnResultSet.get(&quot;ReturningResultSet&quot;);
		if(list.size()&gt;0){
			return (T)list.get(0);
		}else{
			return null;
		}
	}
	
	
	
	/**
	 * 
	 * @功能模块: add
	 * @方法说明: 添加一个对象 将对象t转换成存储过程参数，并执行存储过程，执行存储过程后返回此对象操作后的信息
	 * @version: 1.0
	 * @param &lt;T&gt; 泛类型
	 * @param t 要添加的对象信息
	 * @param procedureName 存储过程名称
	 * @return T 泛类型（返回添加后的对象）
	 * @throws
	 */
	protected &lt;T&gt; T add(T t, String procedureName){
		return this.querySelf(t, procedureName);	
	}
	
	/**
	 * 
	 * @功能模块: add
	 * @方法说明: 添加一个对象 将对象t转换成存储过程参数，并执行存储过程，执行存储过程后返回此对象操作后的信息
	 * @version: 1.0
	 * @param &lt;T&gt; 泛类型
	 * @param t 要添加的对象信息
	 * @return T 泛类型（返回添加后的对象）
	 * @throws
	 */
	public &lt;T&gt; T add(T t){
		return add(t, getProcedureName(t.getClass(), PROCEDURE_NAME_ADD_SUFFIX));			
	}
	
	/**
	 * 
	 * @功能模块: update
	 * @方法说明: 按主键修改一个对象 将对象t转换成存储过程参数，并执行存储过程，执行存储过程后返回此对象操作后的信息
	 * @version: 1.0
	 * @param &lt;T&gt; 泛类型
	 * @param t 要修改的对象信息
	 * @param procedureName 存储过程名称
	 * @return T 泛类型（返回修改后的对象信息）
	 * @throws
	 */
	protected &lt;T&gt; T update(T t, String procedureName){
		return this.querySelf(t, procedureName);			
	}
	
	/**
	 * 
	 * @功能模块: update
	 * @方法说明: 按主键修改一个对象 将对象t转换成存储过程参数，并执行存储过程，执行存储过程后返回此对象操作后的信息
	 * @version: 1.0
	 * @param &lt;T&gt; 泛类型
	 * @param t 要修改的对象信息
	 * @return T 泛类型（返回修改后的对象信息）
	 * @throws
	 */
	public &lt;T&gt; T update(T t){
		return update(t, getProcedureName(t.getClass(), PROCEDURE_NAME_UPDATE_SUFFIX));			
	}
	
	/**
	 * 
	 * @功能模块: delete
	 * @方法说明: 按主键删除一个对象 将对象t转换成存储过程参数，并执行存储过程，执行存储过程后返回执行结果信息
	 * @version: 1.0
	 * @param &lt;T&gt; 泛型
	 * @param t 要删除的对象
	 * @param procedureName 存储过程名称
	 * @return Result 执行结果信息类 
	 * @throws
	 */
	protected &lt;T&gt; Result delete(T t, String procedureName){
		Map returnResultSet = executeProcedure(Result.class, t, procedureName);
		List list = (List)returnResultSet.get(&quot;ReturningResultSet&quot;);
		if(list!=null &amp;&amp; list.size()&gt;0){
			return (Result)list.get(0);
		}else{
			return null;
		}
	}
	
	/**
	 * 
	 * @功能模块: delete
	 * @方法说明: 按主键删除一个对象 将对象t转换成存储过程参数，并执行存储过程，执行存储过程后返回执行结果信息
	 * @version: 1.0
	 * @param &lt;T&gt; 泛型
	 * @param t 要删除的对象
	 * @return Result 执行结果信息类 
	 * @throws
	 */
	public &lt;T&gt; Result delete(T t){
		return delete(t, getProcedureName(t.getClass(), PROCEDURE_NAME_DELETE_SUFFIX));
	}
	
	/**
	 * 
	 * @功能模块: delete
	 * @方法说明: 按主键字符串删除多个对象 将对象t转换成存储过程参数，并执行存储过程，执行存储过程后返回执行结果信息
	 * @version: 1.0
	 * @param ids 要删除的对象ID，多个ID以逗号分割，如：1,2,3
	 * @return Result 执行结果信息类 
	 * @throws
	 */
	protected Result delete(String ids, String procedureName){
		SimpleJdbcCall sjc = this.createSimpleJdbcCall();
		sjc.getJdbcTemplate().setResultsMapCaseInsensitive(true);
		sjc.withProcedureName(procedureName).returningResultSet(&quot;ReturningResultSet&quot;, BeanPropertyRowMapper.newInstance(Result.class));
		
		List list = (List)sjc.execute(ids).get(&quot;ReturningResultSet&quot;);
		if(list!=null &amp;&amp; list.size()&gt;0){
			return (Result)list.get(0);
		}else{
			return null;
		}
	}
	
	/**
	 * 
	 * @功能模块: query
	 * @方法说明: 按条件，排序，分页信息查询内容
	 * @version: 1.0
	 * @param &lt;T&gt; 泛型，查询的对象类型
	 * @param t 对象实例
	 * @param condition 条件
	 * @param order 排序
	 * @param pagination 分页信息
	 * @param procedureName 存储过程名称
	 * @return Map&lt;&quot;Pagination&quot;, PaginationModel&gt;
	 * 		   Map&lt;&quot;List&quot;, List&lt;T&gt;&gt;
	 * @throws
	 */
	@SuppressWarnings(&quot;unchecked&quot;)
	protected Map query(Class clazz, Map&lt;String, Object&gt; condition, String order, PaginationModel pagination, String procedureName){
		SimpleJdbcCall sjc = this.createSimpleJdbcCall();
		sjc.getJdbcTemplate().setResultsMapCaseInsensitive(true);
		sjc.withProcedureName(procedureName).
			returningResultSet(&quot;Pagination&quot;, BeanPropertyRowMapper.newInstance(PaginationModel.class)).
			returningResultSet(&quot;List&quot;, BeanPropertyRowMapper.newInstance(clazz));
		
		Map&lt;String, Object&gt; param = new HashMap&lt;String, Object&gt;();
		param.putAll(condition==null?new HashMap():condition);
		param.put(&quot;orderby&quot;, order==null?&quot;&quot;:order);
		param.put(&quot;pageSize&quot;, pagination.getPageSize());
		param.put(&quot;currPage&quot;, pagination.getCurrPage());
		Map returnResultSet = sjc.execute(param);
		
		Map&lt;String, Object&gt; newResult = new HashMap&lt;String, Object&gt;();
		newResult.put(&quot;List&quot;, returnResultSet.get(&quot;List&quot;));
		
		List list = (List)returnResultSet.get(&quot;Pagination&quot;);
		if(list.size()&gt;0){
			newResult.put(&quot;Pagination&quot;, list.get(0));
		}
		
		return newResult;
	}
	
	/**
	 * 
	 * @功能模块: query
	 * @方法说明: 按条件查询对象列表
	 * @version: 1.0
	 * @param &lt;T&gt; 泛型
	 * @param t 要查询的对象
	 * @param condition 条件
	 * @param procedureName 存储过程名称
	 * @return List 对象列表
	 * @throws
	 */
	@SuppressWarnings(&quot;unchecked&quot;) 
	protected List query(Class clazz, Map&lt;String, Object&gt; condition, String procedureName){
		SimpleJdbcCall sjc = this.createSimpleJdbcCall();
		sjc.getJdbcTemplate().setResultsMapCaseInsensitive(true);
		sjc.withProcedureName(procedureName).
			returningResultSet(&quot;List&quot;, BeanPropertyRowMapper.newInstance(clazz));
		Map resultSet = sjc.execute(condition);
		return (List)resultSet.get(&quot;List&quot;);
	}
	

	/**
	 * 
	 * @功能模块: query
	 * @方法说明: 按对象主键进行查询 将对象t转换成存储过程参数，并执行存储过程进行查询，返回查询后的对象信息
	 * @version: 1.0
	 * @param &lt;T&gt; 泛类型
	 * @param t 要查询的对象信息
	 * @param procedureName 存储过程名称
	 * @return T 执行查询存储过程后返回的对象,与传入对象类型一致
	 * @throws
	 */
	protected &lt;T&gt; T query(T t, String procedureName){
		return this.querySelf(t, procedureName);
	}
	
	/**
	 * 
	 * @功能模块: query
	 * @方法说明: 按对象主键进行查询 将对象t转换成存储过程参数，并执行存储过程进行查询，返回查询后的对象信息
	 * @version: 1.0
	 * @param &lt;T&gt; 泛类型
	 * @param t 要查询的对象信息
	 * @return T 执行查询存储过程后返回的对象,与传入对象类型一致
	 * @throws
	 */
	public &lt;T&gt; T query(T t){
		return query(t, getProcedureName(t.getClass(), PROCEDURE_NAME_QUERY_BY_PRIMARY_KEY_SUFFIX));
	}
	
	/**
	 * 
	 * @功能模块: getProcedureName
	 * @方法说明: 根据查询的对象类，获取此操作此对象存储过程的名称
	 * @version: 1.0
	 * @param clazz 查询的对象类
	 * @param suffix 存储过程后缀
	 * @return String 存储过程名称
	 * @throws
	 */
	private String getProcedureName(Class clazz, String suffix){
		String tableName = TABLE_NAME==null?getTableName(clazz.getSimpleName()):TABLE_NAME;
		return PROCEDURE_NAME_PREFIX + tableName + (suffix==null?&quot;&quot;:(&quot;&quot;.equals(suffix)?&quot;&quot;:&quot;_&quot;+suffix));
	}
	
	/**
	 * 
	 * @功能模块: getTableName
	 * @方法说明: 根据查询的对象类名，获取此对象类对应的表
	 * @version: 1.0
	 * @param className
	 * @return String
	 * @throws
	 */
	private String getTableName(String className){
		if(className.endsWith(&quot;Model&quot;)){			
			String name = className.substring(0, className.length()-&quot;Model&quot;.length());
			return name.endsWith(&quot;Table&quot;)?name:name+&quot;Table&quot;;
		}else{
			return className.endsWith(&quot;Table&quot;)?className:className+&quot;Table&quot;;
		}
	}
}
</pre>
<br>
<br>此BaseDao封装了 添加、删除、修改、查询等基本方法。
<br>其它子DAO继承此BaseDao即可，如有不同覆盖基类方法即可:
<br><pre name="code">
@Transactional
@Repository
public class TestDao extends BaseDao { 
	
	/**
	 * 
	 * @功能模块: selectTestAll
	 * @方法说明: 按分页信息查询内容
	 * @version: 1.0
	 * @param &lt;T&gt; 泛型，查询的对象类型
	 * @param t 对象实例
	 * @param pagination 分页信息
	 * @param procedureName 存储过程名称
	 * @return Map&lt;&quot;Pagination&quot;, PaginationModel&gt;
	 * 		   Map&lt;&quot;List&quot;, List&lt;T&gt;&gt;
	 * @throws
	 */
	public Map selectTestAll(PaginationModel paginationModel) {
		return super.query(TestModel.class, null, null, paginationModel, &quot;st_TestTable_QueryAll&quot;);
	}
	
	/**
	 * 
	 * @功能模块: delete
	 * @方法说明: 按主键字符串删除多个对象 将ids转换成存储过程参数，并执行存储过程，执行存储过程后返回执行结果信息
	 * @version: 1.0
	 * @param ids 要删除的对象ID，多个ID以逗号分割，如：1,2,3
	 * @return Result 执行结果信息类 
	 * @throws
	 */
	public Result delete(String ids){
		return super.delete(ids, &quot;st_TestTable_Delete&quot;);
	}

}
</pre>
<br>
<br>
<br>
<br>
          
  <br><br>
  <ul>
    本文附件下载:
    
      <li><a href="http://dl.javaeye.com/topics/download/fd9724d6-cc67-3244-af53-7c52d7354c2c">TestSpringMVC.rar</a> (8.6 MB)</li>
    
  </ul>

          <br><br>
          作者: <a href="http://joknm.javaeye.com">joknm</a> 
          <br>
          声明: 本文系JavaEye网站发布的原创文章，未经作者书面许可，严禁任何网站转载本文，否则必将追究法律责任！
          <br><br>
          <span style="color:red">
            <a href="http://www.javaeye.com/topic/777732" style="color:red">已有 <strong>0</strong> 人发表回复，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
          </span>
          <br><br><br>
<span style="color:#e28822">JavaEye推荐</span>
<br>
<ul><li><a href="http://www.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li><li><a href="http://www.iteye.com/clicks/439"><span style="color:blue;font-weight:bold">限时报名参加Oracle技术大会</span></a></li><li><a href="http://www.iteye.com/clicks/138"><span style="color:red;font-weight:bold">上海：高薪诚聘php开发人员</span></a></li></ul>
<br><br><br>