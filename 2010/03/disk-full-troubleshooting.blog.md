Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/03/disk-full-troubleshooting/
Post: 63
Title: 磁盘空间满故障排除
Slug: disk-full-troubleshooting
Postformat: standard
Keywords: Linux, Troubleshooting
Status: publish
Date: 2010-03-31 21:19:06 +0800
Pings: On
Comments: On
Category: SysAdmin

**磁盘空间满故障排除**

磁盘空间满一般情况下使用du可以快速定位到那个目录占用了大量的磁盘空间。

这里主要讲两个使用du无法查看的情况。

**现象/mnt分区磁盘使用率达到100%**

<pre lang="bash"># df -h
Filesystem            Size  Used Avail Use% Mounted on
/dev/sda2              97G  1.5G   90G   2% /
/dev/sda1             190M   12M  169M   7% /boot
none                  2.0G     0  2.0G   0% /dev/shm
/dev/sda3              97G  6.3G   85G   7% /usr
/dev/sda6             191G  408M  181G   1% /var
tmpfs                 300M  300M     0 100% /mnt
/dev/loop0            190M  106M   74M  60% /mnt/foo</pre>

**进入/mnt目录使用du查看/mnt下的磁盘使用率**

<pre lang="bash"># cd /mnt
# du -sh *
101M    bar
101M    foo</pre>

**troubleshooting**

解决思路  
有两种情况会干扰du查看磁盘空间使用率  

1. 删除的文件使用du无法查看
2. 磁盘分区的某一个目录挂载了另外一个分区时，du查看到的磁盘空间为挂载分区后的目录空间。

在了解上面两种情况后，解决这个问题会比较简单。  
在生产环境中某一程序的日志文件被删除这一情况发生的几率会大些。  
**故障排除**

<pre lang="bash"># 1、查找被删除文件
# 被删除文件，在写程序未退出的情况下，被删除文件同样会占用磁盘空间。
# lsof -n | head -1
COMMAND     PID     USER   FD      TYPE     DEVICE     SIZE       NODE NAME
# lsof -n /mnt | grep deleted
foo.sh  32593 root    1w   REG   0,18 104538112 981982 /mnt/test.out (deleted)
foo.sh  32593 root    2w   REG   0,18 104538112 981982 /mnt/test.out (deleted)
# 杀掉写文件的程序，磁盘空间会自然释放
# kill 32593
# df -h /mnt
Filesystem            Size  Used Avail Use% Mounted on
tmpfs                 300M  201M  100M  67% /mnt

# 2、查看分区挂载情况
# 因为分区的目录下挂载有其它分区，被挂载分区的目录本身容量无法被查看，所以umount掉挂载分区的目录后将可正常查看此目录下文件所占用的容量。
# cd /mnt
# du -sh *
101M    bar
101M    foo #此容量为目录挂载分区后的新分区容量
# umount /mnt/foo
# du -sh *
21M     bar
201M    foo #此容量为目录所占用磁盘满分区的容量</pre>

✂------✂------✂------✂------✂------✂------✂------✂------✂------✂------

**测试环境搭建过程**

<pre lang="bash"># 挂载300M的内存tmpfs到/mnt目录
# mount -t tmpfs -o size=300m tmpfs /mnt
# cd /mnt && mkdir foo bar
# 先生成两个文件到foo bar下，占用一定的磁盘空间
# dd if=/dev/zero of=foo/file.out bs=1M count=100
# dd if=/dev/zero of=bar/file.out bs=1M count=100   
# 查看磁盘当时的使用情况
# df -h
Filesystem            Size  Used Avail Use% Mounted on
/dev/sda2              97G  1.5G   90G   2% /
/dev/sda1             190M   12M  169M   7% /boot
none                  2.0G     0  2.0G   0% /dev/shm
/dev/sda3              97G  6.3G   85G   7% /usr
/dev/sda6             191G  408M  181G   1% /var
tmpfs                 300M  201M  100M  67% /mnt
# 使用空文件建立一个文件系统
# cd /root/shell
# 生成200M的空文件
# dd if=/dev/zero of=foo.img bs=1M count=200
# 建立一个loop devices
# losetup /dev/loop0 foo.img
# 在loop devices上创建一个ext3文件系统
# mke2fs -j -c /dev/loop0 200000
# 使用/mnt/foo目录挂载/dev/loop0
# mount -t ext3 /dev/loop0 /mnt/foo
# 在/mnt/foo/目录产生一个测试文件file.out
# dd if=/dev/zero of=/mnt/foo/file.out bs=1M count=100
# df -h
Filesystem            Size  Used Avail Use% Mounted on
/dev/sda2              97G  1.5G   90G   2% /
/dev/sda1             190M   12M  169M   7% /boot
none                  2.0G     0  2.0G   0% /dev/shm
/dev/sda3              97G  6.3G   85G   7% /usr
/dev/sda6             191G  408M  181G   1% /var
tmpfs                 300M  201M  100M  67% /mnt
/dev/loop0            190M  106M   74M  60% /mnt/foo
# /mnt/foo目录挂载方式伪装已经完成
# 删除文件伪装
# 编写一个死循环产生测试文件
# cat /root/shell/foo.sh
#!/bin/bash
# set -x
foo=$(seq 1 500)
while :
do
   echo $foo
done
# 生成一个测试文件test.out占用/mnt目录的空间
# nohup /root/shell/foo.sh >/mnt/test.out 2>&1 &
# 删除测试文件
# \rm /mnt/test.out
# df -h
Filesystem            Size  Used Avail Use% Mounted on
/dev/sda2              97G  1.5G   90G   2% /
/dev/sda1             190M   12M  169M   7% /boot
none                  2.0G     0  2.0G   0% /dev/shm
/dev/sda3              97G  6.3G   85G   7% /usr
/dev/sda6             191G  408M  181G   1% /var
tmpfs                 300M  300M     0 100% /mnt
/dev/loop0            190M  106M   74M  60% /mnt/foo</pre>
