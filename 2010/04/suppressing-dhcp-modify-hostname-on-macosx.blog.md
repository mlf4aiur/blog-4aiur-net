Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/04/suppressing-dhcp-modify-hostname-on-macosx/
Post: 141
Title: 防止Mac Os X使用dhcp时修改hostname的方法
Slug: suppressing-dhcp-modify-hostname-on-macosx
Postformat: standard
Keywords: hostname, MacOSX
Status: publish
Date: 2010-04-06 11:29:44 +0800
Pings: On
Comments: On
Category: MacOSX

**防止Mac Os X使用dhcp时修改hostname的方法**

在/etc/hostconfig中增加HOSTNAME项，这样可以阻止使用dhcp时，hostname经常被更改的问题。

使用命令行的方式修改

<pre lang="bash">scutil --set HostName new_hostname
sudo hostname new_hostname</pre>

或者直接编辑配置文件
<pre lang="bash">[4aiur@localhost ~]$ sudo vi /etc/hostconfig
HOSTNAME=4Aiur</pre>

在桌面上修改hostname的方法:

1. Launch ‘System Preferences’
2. Click the ‘Sharing’ icon
3. Type in what you want your Mac’s new computer name to be
