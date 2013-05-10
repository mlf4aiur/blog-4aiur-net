Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/03/shell-recursive-example/
Post: 80
Title: shell递归一例
Slug: shell-recursive-example
Postformat: standard
Keywords: shell
Status: publish
Date: 2010-03-31 21:42:48 +0800
Pings: On
Comments: On
Category: Shell

<p>用递归写的一个光标旋转的小脚本<br />
	[root@maint-app-108 recurse]# cat cursor.sh</p>
<pre lang="bash">cursor () { #{{{
   message="$1"
   count="$2"
   for item in "|" "/" "-" "\\"
   do
       printf "\r${item} ${message} $count"
       usleep 50000
   done
   cursor waiting $((count+1))
}

cursor waiting 1</pre>
<p>[root@maint-app-108 cursor]# 用递归做这种无限循环最后脚本会退出，下面这个while :不会退出。 [root@maint-app-108 cursor]# cat cursor.sh</p>
<pre lang="bash">cursor () { #{{{
   message="$1"
   for item in "|" "/" "-" "\\"
   do
       printf "\r${item} ${message}"
       usleep 50000
   done
}

while :
do
   cursor waiting
done</pre>
<br />
