---
layout: post
title:  "Quicklisp 介紹"
date:   2013-05-10 12:11:09
author: 
categories: program
---

## Quicklisp 介紹
### by 
### at 2013-05-10 12:11:09
### original <http://lisp.tw//2013/05/10/quicklisp-intro/>

<p><span>
作者：Lisp Taiwan
<span><a href="http://quicklisp.org/">http://quicklisp.org/</a></span>
</span></p>
<img src="http://lisp.tw/images/quicklisp.png" title="Quicklisp, Common Lisp 軟件包管理工具">

<span></span>

<p>由 <a href="http://www.xach.com/">Zach Beane</a> 所開發的 <a href="http://quicklisp.org/">Quicklisp</a> 是一個 Common Lisp 的函式庫管理工具，只需要幾個簡單的命令，就可以自動下載、安裝、載入超過 <a href="http://www.quicklisp.org/beta/releases.html">700+ 個軟件包</a>。如果要類比的話，Quicklisp 就像是 apt-get 之於 Ubuntu、Homebrew 之於 Mac、Gem 之於 Ruby、NPM 之於 Node.js。</p>
<p>以前可能有人使用過 ASDF-INSTALL，但這個軟件已經<strong>沒有人再維護了，絕對不要再用了。</strong></p>
<p>目前 Quicklisp 可以在 ABCL、Allegro CL、Clozure CL、CLISP、CMUCL、ECL、LispWorks、SBCL、以及 Scieneer CL 上使用。</p>
<p>所有被納入 Quicklisp 的函式庫皆經過相容性與依賴性測試（由 zach 本人測試），確保它們在主流的 Common Lisp 實現上是可以工作的，並且與其它的函式庫不相互衝突。</p>
<h2>安裝 Quicklisp</h2>
<p>步驟很簡單，只要下載 quicklisp.lisp、加載進來、安裝、加入啟動文件。之後就可以輕鬆加載第三方函式庫摟。</p>
<p>系統：Mac OSX</p>
<p>Common Lisp 實現：SBCL</p>
<p><code>$</code> 號為命令行、<code>?</code> 為 Lisp REPL。</p>
<table><tr><td><pre><code><div>1</div><div>2</div><div>3</div><div>4</div></code></pre></td><td><pre><code><div><span>$ </span>curl -<span>O</span> <span>http:</span>//beta.quicklisp.org/quicklisp.lisp</div><div><span>$ </span>sbcl --load quicklisp.lisp</div><div>* (quicklisp-<span>quickstart:</span>install)</div><div>* (<span>ql:</span>add-to-init-file)</div></code></pre></td></tr></table>

<h2>安裝過了，再次加載？</h2>
<p>如果你沒有執行上面的 <code>(ql:add-to-init-file)</code>，將會需要再每次啟動 Common Lisp 之後手動加載 Quicklisp。</p>
<table><tr><td><pre><code><div>1</div></code></pre></td><td><pre><code><div><span>(<span>load</span><span> <span>"~/quicklisp/setup.lisp"</span>)</span></span></div></code></pre></td></tr></table>

<p><strong>注意!!!!</strong></p>
<p><strong>將路徑換成 quicklisp 所在的目錄，CMUCL 不支持解析 <code>~</code> 家目錄</strong></p>
<h2>安裝特定版本的軟件包？</h2>
<p>參考這篇文章：<a href="http://blog.quicklisp.org/2011/08/going-back-in-dist-time.html">Going back in dist time</a></p>
<h2>Quicklisp 命令</h2>
<table>
  <tr>
    <th>Command</th><th>Description</th><th></th>
  </tr>
  <tr>
    <td><code>(ql:quickload system-name)</code></td><td>自動下載軟件包</td>
  </tr>
  <tr>
    <td><code>(ql:system-apropos substring)</code></td><td>搜索帶有 <code>substring</code> 的軟件包</td>
  </tr>
  <tr>
    <td><code>(ql:add-to-init-file)</code></td><td>啟動時自動加載 Quicklisp</td>
  </tr>
  <tr>
    <td><code>(ql:quickload &quot;quicklisp-slime-helper&quot;)</code></td><td>安裝提供給 SLIME 的 Quicklisp 助手</td>
  </tr>
  <tr>
    <td><code>(ql:update-all-dists)</code></td><td>獲取第三方軟件包更新</td>
  </tr>
  <tr>
    <td><code>(ql:update-client)</code></td><td>更新 Quicklisp</td>
  </tr>
  <tr>
    <td><code>(ql:who-depends-on system-name)</code></td><td>看特定軟件包的依賴關係</td>
  </tr>
</table>

<h2>尋求幫助</h2>
<p>最新消息：在推特上跟隨 <a href="http://twitter.com/quicklisp">@quicklisp</a> 或閱讀 <a href="http://blog.quicklisp.org/">Quicklisp 的 blog</a>。</p>
<p>有任何問題或意見請<a href="mailto:zach@quicklisp.org">寄信給 zach</a>，或到 freenode 的 #quicklisp 頻道。</p>
<p>Quicklisp 代碼放在 <a href="http://github.com/quicklisp/">GitHub</a></p>
<h2>延伸閱讀</h2>
<p><a href="http://www.quicklisp.org/beta/faq.html">Quciklisp FAQ</a></p>