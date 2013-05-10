Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/04/create-yum-repository/
Post: 143
Title: 创建本地yum源服务器
Slug: create-yum-repository
Postformat: standard
Keywords: Linux, yum
Status: publish
Date: 2010-04-06 11:31:44 +0800
Pings: On
Comments: On
Category: Linux

**创建本地yum源服务器**

<pre lang="bash"># 安装createrepo包
[root@localhost centos]# yum -y install createrepo
[root@localhost centos]# cat update.sh 
#!/bin/bash
# set -x
 
rsync --progress -rvu -lptD --exclude="isos/" --exclude="SRPMS/" --exclude="x86_64" rsync://rsync.kddilabs.jp/centos/5.4/ mirror/
createrepo -v --update mirror/myapp/i386/
 
# 同步本地yum库
[root@localhost centos]# ./update.sh</pre>

### 生成yum配置文件提供客户端使用

<pre lang="bash">[root@localhost foo]# cat myyumsource.repo
# log.repo
[myapp]
name=CentOS-$releasever - myapp
baseurl=http://192.168.3.63:9999/mirror/myapp/i386/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-5

[base]
name=CentOS-$releasever - Base
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras
#baseurl=http://mirror.centos.org/centos/$releasever/os/$basearch/
baseurl=http://192.168.3.63:9999/mirror/os/i386/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-5

#released updates 
[updates]
name=CentOS-$releasever - Updates
baseurl=http://192.168.3.63:9999/mirror/updates/i386
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-5

#packages used/produced in the build but not released
[addons]
name=CentOS-$releasever - Addons
baseurl=http://192.168.3.63:9999/mirror/addons/i386
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-5

#additional packages that may be useful
[extras]
name=CentOS-$releasever - Extras
baseurl=http://192.168.3.63:9999/mirror/extras/i386
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-5

#additional packages that extend functionality of existing packages
[centosplus]
name=CentOS-$releasever - Plus
baseurl=http://192.168.3.63:9999/mirror/centosplus/i386
gpgcheck=1
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-5

#contrib - packages by Centos Users
[contrib]
name=CentOS-$releasever - Contrib
baseurl=http://192.168.3.63:9999/mirror/contrib/i386
gpgcheck=1
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-5</pre>

### yum客户端常用命令

<pre lang="bash">yum update # 升级本地程序
yum install # 使用rpm安装软件
yum clean all # 清除yum缓存在本地的文件，当yum发生问题时可以尝试使用clean来清理掉本地缓存中的文件</pre>

查找rpm资源的地址：<http://dries.ulyssis.org/rpm/>  
可以在这里找到很多编译rpm包的spec源文件。  
可以借鉴其它的spec文件来编写自己的spec文件
