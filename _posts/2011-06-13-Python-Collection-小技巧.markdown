---
layout: post
title:  "Python Collection 小技巧"
date:   2011-06-13 22:50:47
author: 黑日白月
categories: program
---

## Python Collection 小技巧
### by 黑日白月
### at 2011-06-13 22:50:47
### original <http://linuxtoy.org/archives/python-collection-tips.html>

<p><iframe src="http://feedads.g.doubleclick.net/~ah/f/r45t08ks0fj6sr7aa7q8jurtt8/300/250?ca=1&amp;fh=280#http%3A%2F%2Flinuxtoy.org%2Farchives%2Fpython-collection-tips.html" width="100%" height="280" frameborder="0" scrolling="no" marginwidth="0" marginheight="0"></iframe></p><p>独立软件开发者 Alex Marandon 在他的博客中介绍了数个关于 Python Collection 的实用小技巧，在此与诸位分享。<span></span></p>

<p><strong>判断一个 list 是否为空</strong></p>

<p>传统的方式：</p>

<pre><code>if len(mylist):
    # Do something with my list
else:
    # The list is empty</code></pre>

<p>由于一个空 list 本身等同于 <code>False</code>，所以可以直接：</p>

<pre><code>if mylist:
    # Do something with my list
else:
    # The list is empty</code></pre>

<p><strong>遍历 list 的同时获取索引</strong></p>

<p>传统的方式：</p>

<pre><code>i = 0
for element in mylist:
    # Do something with i and element
    i += 1</code></pre>

<p>这样更简洁些：</p>

<pre><code>for i, element in enumerate(mylist):
    # Do something with i and element
    pass</code></pre>

<p><strong>list 排序</strong></p>

<p>在包含某元素的列表中依据某个属性排序是一个很常见的操作。例如这里我们先创建一个包含 person 的 list：</p>

<pre><code>class Person(object):
    def __init__(self, age):
        self.age = age

persons = [Person(age) for age in (14, 78, 42)]</code></pre>

<p>传统的方式是：</p>

<pre><code>def get_sort_key(element):
    return element.age

for element in sorted(persons, key=get_sort_key):
    print "Age:", element.age</code></pre>

<p>更加简洁、可读性更好的方法是使用 Python 标准库中的 <code>operator</code> 模块：</p>

<pre><code>from operator import attrgetter

for element in sorted(persons, key=attrgetter('age')):
    print "Age:", element.age</code></pre>

<p><code>attrgetter</code> 方法优先返回读取的属性值作为参数传递给 <code>sorted</code> 方法。<code>operator</code> 模块还包括 <code>itemgetter</code> 和 <code>methodcaller</code> 方法，作用如其字面含义。</p>

<p><strong>在 Dictionary 中元素分组</strong></p>

<p>和上面类似，先创建 Persons：</p>

<pre><code>class Person(object):
    def __init__(self, age):
        self.age = age

persons = [Person(age) for age in (78, 14, 78, 42, 14)]</code></pre>

<p>如果现在我们要按照年龄分组的话，一种方法是使用 <code>in</code> 操作符：</p>

<pre><code>persons_by_age = {}

for person in persons:
    age = person.age
    if age in persons_by_age:
        persons_by_age[age].append(person)
    else:
        persons_by_age[age] = [person]

assert len(persons_by_age[78]) == 2</code></pre>

<p>相比较之下，使用 <code>collections</code> 模块中 <code>defaultdict</code> 方法的途径可读性更好：</p>

<pre><code>from collections import defaultdict

persons_by_age = defaultdict(list)

for person in persons:
    persons_by_age[person.age].append(person)</code></pre>

<p><code>defaultdict</code> 将会利用接受的参数为每个不存在的 key 创建对应的值，这里我们传递的是 <code>list</code>，所以它将为每个 key 创建一个 <code>list</code> 类型的值。</p>

<p><em>内容来源：</em><a href="http://alexmarandon.com/articles/python_collections_tips/">AlexMarandon.com</a></p>
	<p></p>
	<p>分类: <a href="http://linuxtoy.org/category/featured-post" title="View all posts in Featured" rel="category tag">Featured</a>,  <a href="http://linuxtoy.org/category/tips" title="View all posts in Tips" rel="category tag">Tips</a> | 
	<a href="http://linuxtoy.org/archives/python-collection-tips.html">永久链接</a> |
	<a href="http://delicious.com/save?url=http://linuxtoy.org/archives/python-collection-tips.html&amp;title=Python%20Collection%20%E5%B0%8F%E6%8A%80%E5%B7%A7">收藏到 del.icio.us</a> | 
	<a href="mailto:?Subject=Check+This+Out&amp;body=I+think+you&#39;ll+like+this:+http://linuxtoy.org/archives/python-collection-tips.html">Email 给好友</a> | 
    <a href="http://linuxtoy.org/archives/python-collection-tips.html#comments">38 评论</a> |
    <a href="http://linuxtoy.org/faq/donate">捐助本站</a></p>