Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/04/freebsd-memory-file-system/
Post: 111
Title: FreeBSD内存文件系统
Slug: freebsd-memory-file-system
Postformat: standard
Keywords: FreeBSD
Status: publish
Date: 2010-04-06 11:03:35 +0800
Pings: On
Comments: On
Category: BSD

**FreeBSD内存文件系统**

<pre lang="bash">[4aiur@FreeBSD ~/memeory_fs]$ sudo mount -t tmpfs -o size=1m tmpfs tmpfs/
[4aiur@FreeBSD ~/memeory_fs]$ sudo mdmfs -s 1m md mfs
[4aiur@FreeBSD ~/memeory_fs]$ df -h tmpfs/ mfs/
Filesystem     Size    Used   Avail Capacity  Mounted on
tmpfs          648M    4.0K    648M     0%    /usr/home/4aiur/memeory_fs/tmpfs
/dev/md5       846K    4.0K    776K     1%    /usr/home/4aiur/memeory_fs/mfs</pre>
