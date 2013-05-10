Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/06/test-network-performance-without-write-disk/
Post: 193
Title: 在不浪费磁盘空间的情况下测试网络
Slug: test-network-performance-without-write-disk
Postformat: standard
Status: publish
Date: 2010-06-09 09:36:38 +0800
Pings: Off
Comments: On
Category: SysAdmin

**在不浪费磁盘空间的情况下测试网络**

<pre lang="bash">time dd if=/dev/zero bs=1024 count=1048576 | ssh user@remotehost 'cat > /dev/null'</pre>

在数据传输过程中可以使用iptraf进行观察
