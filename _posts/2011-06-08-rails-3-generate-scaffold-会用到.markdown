---
layout: post
title:  "rails 3 generate scaffold 会用到"
date:   2011-06-08 22:49:32
author: 
categories: program
---

## rails 3 generate scaffold 会用到
### by 
### at 2011-06-08 22:49:32
### original <http://hlee.iteye.com/blog/1073636>

一个可能要知道model的数据格式都支持什么
<br><a href="http://hlee.iteye.com/admin/blogs/1015268">可以支持的数据类型</a>
<br>
<br>一个是如果要是创建完了，发现不满意怎么办呢
<br>
<br>例如，运行如下：
<br><pre name="code">
rails g scaffold task project_id:integer name:string
</pre>
<br>
<br>我的话，第一想到的是git checkout或者git reset
<br>实际上，用如下更合适
<br>
<br><pre name="code">
rails destroy scaffold task
</pre>
<br>
<br>那么，要是生成了scaffold并且，用了一段了时间了呢
<br>
<br>可以先把数据库版本退回来
<br><pre name="code">rake db:rollback STEP=3</pre>
<br>
<br>当然，通常有数据就不建议rollback了，就要
<br>
<br>就要<pre name="code">
rails generate migration AddPartNumberToProducts part_number:string
</pre>
<br><pre name="code">
class AddPartNumberToProducts &lt; ActiveRecord::Migration
  def self.up
    add_column :products, :part_number, :string
  end
 
  def self.down
    remove_column :products, :part_number
  end
end
</pre>
<br>
<br><pre name="code">
rails generate migration RemovePartNumberFromProducts part_number:string
</pre><pre name="code">
class RemovePartNumberFromProducts &lt; ActiveRecord::Migration
  def self.up
    remove_column :products, :part_number
  end
 
  def self.down
    add_column :products, :part_number, :string
  end
end
</pre>
<br>
<br><pre name="code">
class ExampleMigration &lt; ActiveRecord::Migration
 
  def self.up
    create_table :products do |t|
      t.references :category
    end
    #add a foreign key
    execute &lt;&lt;-SQL
      ALTER TABLE products
        ADD CONSTRAINT fk_products_categories
        FOREIGN KEY (category_id)
        REFERENCES categories(id)
    SQL
 
    add_column :users, :home_page_url, :string
 
    rename_column :users, :email, :email_address
  end
 
  def self.down
    rename_column :users, :email_address, :email
    remove_column :users, :home_page_url
    execute &quot;ALTER TABLE products DROP FOREIGN KEY fk_products_categories&quot;
    drop_table :products
  end
end
</pre>
<br>
<br>
<br>
<br>
<br>参考：
<br><a href="http://asciicasts.com/episodes/216-generators-in-rails-3">http://asciicasts.com/episodes/216-generators-in-rails-3</a>
<br><a href="http://guides.rubyonrails.org/migrations.html">http://guides.rubyonrails.org/migrations.html</a>
<br><a href="http://www.viget.com/extend/rails-3-generators-the-old-faithful/">http://www.viget.com/extend/rails-3-generators-the-old-faithful/</a>
<br>
              
              <br><br>
              <span style="color:red">
                <a href="http://hlee.iteye.com/blog/1073636#comments" style="color:red">已有 <strong>1</strong> 人发表留言，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
              </span>
              <br><br><br>
<span style="color:#e28822">ITeye推荐</span>
<br>
<ul><li><a href="http://hlee.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>