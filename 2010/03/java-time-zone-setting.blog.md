Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/03/java-time-zone-setting/
Post: 72
Title: 设置JAVA时区
Slug: java-time-zone-setting
Postformat: standard
Keywords: Java
Status: publish
Date: 2010-03-31 21:30:37 +0800
Pings: On
Comments: On
Category: Linux

**设置JAVA时区**

RadHat上面运JDK，其获取时区的配置文件是/etc/sysconfig/clock。

<pre lang="bash">
# cat /etc/sysconfig/clock
ZONE="Asia/Shanghai"
UTC=false
ARC=false</pre>

昨天遇到了一个很怪异的现象。

现象是java程序输出的时间和系统时间相差了13个小时，与<http://www.javaeye.com/topic/173077>现象相同。

使用data命令查看系统时区是CST，但是执行java程序输出的取是"America/New_York"

使用timeconfig重新设置系统时区后，java获取到的时区恢复正常。

看了下timeconfig的manual，发现这个命令配置两个文件，分别是/etc/sysconfig/clock、/etc/localtime。

data命令输出的时区与java时区有差异就是因为它们读取的配置文件不同。
