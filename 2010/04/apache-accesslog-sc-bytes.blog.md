Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/04/apache-accesslog-sc-bytes/
Post: 139
Title: 关于apache日志中的sc-bytes
Slug: apache-accesslog-sc-bytes
Postformat: standard
Keywords: accesslog, Apache
Status: publish
Date: 2010-04-06 11:29:15 +0800
Pings: On
Comments: On
Category: SysAdmin

**关于apache日志中的sc-bytes**

apache官方manual中描述的%b字段应该是服务器端发送到客户端的字节数，今天做了一个测试发现写入到日志中的bytes并不是真正的sc-bytes，而是response中的Content-Length。

手册中的描述如下:  
%b Size of response in bytes, excluding HTTP headers. In CLF format, i.e. a '-' rather than a 0 when no bytes are sent.

测试方法如下：服务器端生成一个大文件

<pre lang="bash"># dd if=/dev/zero of=foo bs=1M count=300300+0 records in
300+0 records out# ll -h foo
-rw-r--r-- 1 root root 300M Mar 22 17:36 foo</pre>

客户段使用wget抓取这个文件，使用参数-d来查看详细情况。在wget开始抓取文件后，停止wget运行，查看文件大小是2.5M，而apache的日志中的sc-bytes记录的值确是314572800。  
结论：使用apache的日志计算流量，结果将不准确。

<pre lang="bash">[root@host ~]# wget -d http://*.*.*.*/fooDEBUG output created by Wget 1.10.2 (Red Hat modified) on linux-gnu.

--18:01:20-- http://*.*.*.*/foo => 'foo'
Connecting to *.*.*.*:80… connected.Created socket 3.
Releasing 0x09c85bc8 (new refcount 0).Deleting unused 0x09c85bc8.

—request begin—GET /foo HTTP/1.0
User-Agent: Wget/1.10.2 (Red Hat modified)Accept: */*
Host: *.*.*.*Connection: Keep-Alive

—request end—HTTP request sent, awaiting response…
—response begin—HTTP/1.1 200 OK
Date: Mon, 22 Mar 2010 09:48:30 GMTServer: Apache/2.2.13 (Unix) mod_ssl/2.2.13 OpenSSL/0.9.7a
Last-Modified: Mon, 22 Mar 2010 09:36:47 GMTETag: "3e88e1-12c00000-482606f8fa9c0"
Accept-Ranges: bytesContent-Length: 314572800
Keep-Alive: timeout=5, max=100Connection: Keep-Alive
Content-Type: text/plain

—response end—200 OK
Registered socket 3 for persistent reuse.Length: 314,572,800 (300M) [text/plain]

0% [ ] 2,303,436 832.01K/s
[root@host ~]# ll -h foo
-rw-r--r-- 1 root root 2.5M Mar 22 18:01 foo
[root@host ~]# \rm foo

# tail -f access_log | grep foo
*.*.*.* - - [22/Mar/2010:17:48:30 +0800] "GET /foo HTTP/1.0" 200 314572800 "-" "Wget/1.10.2 (Red Hat modified)"</pre>
