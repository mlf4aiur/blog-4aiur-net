Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/04/restart-netwok-on-freebsd/
Post: 104
Title: FreeBSD重启网卡
Slug: restart-netwok-on-freebsd
Postformat: standard
Keywords: FreeBSD
Status: publish
Date: 2010-04-06 11:01:28 +0800
Pings: On
Comments: On
Category: BSD

**FreeBSD重启网卡**

<pre lang="bash">
# 本地重启：
# ifconfig le0 down  //stop网卡
# ifconfig le0 up  //start网卡

# 远程重启：
#/etc/netstart
#sh /etc/rc
#/etc/rc.d/netif restart
# 重新获取dhcp地址：
/sbin/dhclient interface</pre>
