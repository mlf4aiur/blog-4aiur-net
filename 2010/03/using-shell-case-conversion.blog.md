Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/03/using-shell-case-conversion/
Post: 31
Title: 使用shell进行大小写转换
Slug: using-shell-case-conversion
Postformat: standard
Keywords: shell
Status: publish
Date: 2010-03-31 17:30:28 +0800
Pings: On
Comments: On
Category: Shell

**使用shell进行大小写转换**

大写转小写
<pre lang="bash">for f in *; do rename $f `echo $f | tr "[:upper:]" "[:lower:]"` $f; done</pre>

小写转大写
<pre lang="bash">for f in *; do rename $f `echo $f | tr "[:lower:]" "[:upper:]"` $f; done</pre>
