---
layout: post
title:  "rails 3 undefined method `model_name' for ActiveRecord::Relation:Class."
date:   2011-06-09 10:19:47
author: 
categories: program
---

## rails 3 undefined method `model_name' for ActiveRecord::Relation:Class.
### by 
### at 2011-06-09 10:19:47
### original <http://hlee.iteye.com/blog/1073937>

rails 3 form_for 的问题
<br>
<br>undefined method `model_name' for ActiveRecord::Relation:Class.
<br>
<br><pre name="code">
def edit
   @office = Office.where("id = ? AND company_id = ?", params[:id], @company.id )
end
</pre>
<br><pre name="code">
&lt;%= simple_form_for @office, :url =&gt; settings_office_path, :html =&gt; { :class =&gt; &quot;office_form&quot; } do |f| %&gt;
  &lt;h1&gt;Edit &lt;%= @office.office_name %&gt; Details&lt;/h1&gt;
  &lt;%= render :partial =&gt; &#39;form&#39;, :locals =&gt; { :f =&gt; f } %&gt;
&lt;% end %&gt;
</pre>
<br>
<br>需要修改
<br>
<br><pre name="code">@office = Office.where(:company_id =&gt; @company.id).find(params[:id])

@office = @company.offices.find(params[:id])</pre>
<br>
<br>
<br>下面有个人和我类似
<br>
<br><pre name="code">class Event &lt; ActiveRecord::Base
  belongs_to :user

  attr_accessible :event_name, :Starts_at, :finish, :tracks
end</pre>
<br>
<br><pre name="code">class User &lt; ActiveRecord::Base
  has_many :events, :dependent =&gt; :destroy

  attr_accessible :name, :event_attributes

  accepts_nested_attributes_for :events,  :allow_destroy =&gt; true

end</pre>
<br>
<br><pre name="code">ActiveRecord::Schema.define(:version =&gt; 20101201180355) do

  create_table &quot;events&quot;, :force =&gt; true do |t|

    t.string   &quot;event_name&quot;
    t.string   &quot;tracks&quot;
    t.datetime &quot;starts_at&quot;
    t.datetime &quot;finish&quot;
    t.datetime &quot;created_at&quot;
    t.datetime &quot;updated_at&quot;
    t.integer  &quot;user_id&quot;
  end
end</pre>
<br><div>引用</div><div>
<br>NoMethodError in Users#index 
<br>undefined method `events&#39; for #&lt;ActiveRecord::Relation:0x4177518&gt;
<br>
<br>Extracted source (around line #10):
<br>
<br>7:  &lt;th&gt;&lt;%= sortable &quot;Tracks&quot; %&gt;&lt;/th&gt;
<br>
<br>8:   &lt;/tr&gt;
<br>
<br>
<br>10:  &lt;% @users.events.each do |event| %&gt; 
<br>
<br>11: &lt;% debugger %&gt; 
<br>
<br>12:   &lt;tr&gt;
<br>
<br>13:     &lt;td&gt;&lt;%= event.starts_at %&gt;&lt;/td&gt;
<br>
<br>Trace of template inclusion: app/views/users/index.html.erb
<br>
<br>Rails.root: C:/rails_project1/events_manager
<br>Application Trace | Framework Trace | Full Trace
<br>
<br>app/views/users/_event_user.html.erb:10:in `_app_views_users__event_user_html_erb__412443848_34308540_1390678'
<br>app/views/users/index.html.erb:7:in `_app_views_users_index_html_erb___603337143_34316016_0'
<br></div>
<br>
<br>
<br>问题解决：
<br>
<br><pre name="code">@users = User.find(&lt;conditions go here&gt;)
#要变成

@users = User.where(&lt;conditions go here&gt;).all
#或者如下

@users = User.where(:admin =&gt; true).where(&#39;created_at &gt; ?&#39;, min_date).order(&#39;created_at&#39;).limit(10).all</pre>
<br>
              
              <br><br>
              <span style="color:red">
                <a href="http://hlee.iteye.com/blog/1073937#comments" style="color:red">已有 <strong>0</strong> 人发表留言，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
              </span>
              <br><br><br>
<span style="color:#e28822">ITeye推荐</span>
<br>
<ul><li><a href="http://hlee.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>