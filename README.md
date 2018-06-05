---


---

<h1 id="development-issues-summary">Development-issues-summary</h1>
<h2 id="开发中遇到的一些问题和解决方案的合集">开发中遇到的一些问题和解决方案的合集</h2>
<ol>
<li>List item使用material desgin显示时，遇到IllegalArgumentException。<pre><code> Caused by: java.lang.IllegalArgumentException: 
       AppCompat does not support the current theme features:{
	          windowActionBar: false,
	          windowActionBarOverlay: false,
	          android:windowIsFloating: false, 
	          windowActionModeOverlay: false, 
	          windowNoTitle: false } ;
</code></pre>
<blockquote>
<p>解决办法：<br>
（1）在style文件中将  <code>&lt;item name="android:windowNoTitle"&gt;true&lt;/item&gt;</code>  改为<code>&lt;item name="windowNoTitle"&gt;true&lt;/item&gt;</code><br>
（2）在values-v21文件夹下面 添加一个新的styles.xml配置文件:<code>&lt;style name="AppTheme" parent="AppTheme.Base"&gt;</code>,在values文件下的styles文件中多添加一层继承的style标签。<br>
具体详见  <a href="http://blog.csdn.net/woshishui5577/article/details/53285351" title="悬停显示">我的博客</a></p>
</blockquote>
</li>
<li></li>
</ol>

