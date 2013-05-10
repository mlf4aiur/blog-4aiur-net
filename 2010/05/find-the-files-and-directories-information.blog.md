Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/05/find-the-files-and-directories-information/
Post: 166
Title: 查找使用中的文件与目录相关信息
Slug: find-the-files-and-directories-information
Postformat: standard
Keywords: shell
Status: publish
Date: 2010-05-10 10:37:33 +0800
Pings: Off
Comments: On
Category: SysAdmin

**查找使用中的文件与目录相关信息**

以tmp目录为例，使用下面得方法获得使用tmp目录进程的相关信息。

<pre lang="bash">
[root@4aiur ~]# inode=`stat -c %i tmp`
[root@4aiur ~]# cd tmp
[root@4aiur ~]# lsof -n | awk -v a=$inode 'a==$(NF-1) || a==$(NF-2)'
bash       1963    root  cwd       DIR      253,0     4096   23805953 /root/tmp
lsof       2033    root  cwd       DIR      253,0     4096   23805953 /root/tmp
awk        2034    root  cwd       DIR      253,0     4096   23805953 /root/tmp
lsof       2035    root  cwd       DIR      253,0     4096   23805953 /root/tmp</pre>
