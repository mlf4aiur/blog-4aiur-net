Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/04/print-fifty-dash/
Post: 119
Title: 使用多种方法打印50个连续的横杠
Slug: print-fifty-dash
Postformat: standard
Keywords: shell
Status: publish
Date: 2010-04-06 11:19:21 +0800
Pings: On
Comments: On
Category: SysAdmin

**使用多种方法打印50个连续的横杠**

<pre lang="bash">python -c 'print "-" * 50'
perl -le'print"-"x50'
awk 'BEGIN{while (a++<50) s=s "-"; print s}'
jot -s '' -b '-' 50
echo - | sed -e :a -e 's/^.\{1,50\}$/&-/;ta'
In bash, by pressing ALT+n and then a character x, x will be printed n times
# I know is not the same as the original command, but is correlated.
# ALT-50 -
</pre>
