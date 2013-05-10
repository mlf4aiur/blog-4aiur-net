Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/05/convert-unix-timestamp/
Post: 168
Title: unix时间戳转换
Slug: convert-unix-timestamp
Postformat: standard
Keywords: script
Status: publish
Date: 2010-05-14 14:44:37 +0800
Pings: Off
Comments: On
Category: SysAdmin

**unix时间戳转换, Convert unix timestamp**

<pre lang="bash">
utime(){ date -d @$1; }
utime(){ date -d "1970-01-01 GMT $1 seconds"; }
utime(){ awk -v d=$1 'BEGIN{print strftime("%a %b %d %H:%M:%S %Y", d)}'; }
utime(){ perl -e "print localtime($1).\"\n\""; }
utime(){ python -c "import time; print(time.strftime('%a %b %d %H:%M:%S %Y', time.localtime($1)))"; }

[root@test test]# utime(){ date -d @$1; }
[root@test test]# utime 1273817956
Fri May 14 14:19:16 CST 2010

[root@test test]# utime 1273817956
Fri May 14 14:19:16 CST 2010

[root@test test]# utime(){ awk -v d=$1 'BEGIN{print strftime("%a %b %d %H:%M:%S %Y", d)}'; }
[root@test test]# utime 1273817956
Fri May 14 14:19:16 2010

[root@test test]# utime(){ perl -e "print localtime($1).\"\n\""; }
[root@test test]# utime 1273817956
Fri May 14 14:19:16 2010

[root@test test]# utime(){ python -c "import time; print(time.strftime('%a %b %d %H:%M:%S %Y', time.localtime($1)))"; }
[root@test test]# utime 1273817956
Fri May 14 14:19:16 2010
[root@test test]#
</pre>
