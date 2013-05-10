Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/03/delete-special-characters-in-filename/
Post: 68
Title: 带特殊字符文件的删除方法
Slug: delete-special-characters-in-filename
Postformat: standard
Keywords: Linux, shell
Status: publish
Date: 2010-03-31 21:22:23 +0800
Pings: On
Comments: On
Category: Linux

**带特殊字符文件的删除方法**

list文件列表的时候发现有个"?"文件，直接删除?是删不掉的。

<pre lang="bash">
~]# ll
total 75040
-rw-r--r--   1 root root        0 Nov  7 14:46 ?
# 使用cat -A 查看这个文件是带有特殊字符的文件
~]# ll | cat -A
total 75040$
-rw-r--r--   1 root root        0 Nov  7 14:46 M-<$
~]# ls | head -1
¼
 [
# 使用下面的方法将其删除
# ls | head -1 | xargs rm</pre>
