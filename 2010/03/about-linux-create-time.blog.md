Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/03/about-linux-create-time/
Post: 66
Title: 关于linux文件的创建时间
Slug: about-linux-create-time
Postformat: standard
Keywords: Linux, MacOSX
Status: publish
Date: 2010-03-31 21:21:20 +0800
Pings: On
Comments: On
Category: Linux

**关于linux文件的创建时间**

inux的ctime代表的是文件修改时间，如果文件被修改过就很难知道文件的创建时间，在某些特殊情况下，需要查看文件的创建时间，正常情况下查看文件的ctime是无法实现的。

可以使用一个变通的方法来实现保留文件创建时间，但是同时也会牺牲一些其它特性。

以下方法可以部分方法造成的文件状态修改问题

文件的状态有atime,ctime,mtime，可以在mount文件的时候使用参数-o noatime，来把系统更新atime的特性关闭。

使用了noatime参数挂载后，在文件被修改后文件的atime是不会被改变的，使用stat查看到的atime就是文件的创建时间。

实验如下：

<pre lang="bash">
# mount -t ext3 -o noatime /dev/loop0 /mnt/foo
# touch file
# stat file
 File: 'file'
 Size: 0               Blocks: 2          IO Block: 4096   regular empty file
Device: 700h/1792d      Inode: 13          Links: 1
Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2008-11-26 19:00:46.000000000 +0800
Modify: 2008-11-26 19:00:46.000000000 +0800
Change: 2008-11-26 19:00:46.000000000 +0800
# echo foo >> file
# stat file
 File: 'file'
 Size: 4               Blocks: 4          IO Block: 4096   regular file
Device: 700h/1792d      Inode: 13          Links: 1
Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2008-11-26 19:00:46.000000000 +0800
Modify: 2008-11-26 19:09:56.000000000 +0800
Change: 2008-11-26 19:09:56.000000000 +0800</pre>

通过以上实验可以看出文件的access time 是不变的。

此方法基本无使用价值，既然已经写了就不删除了，留做纪念算了。
