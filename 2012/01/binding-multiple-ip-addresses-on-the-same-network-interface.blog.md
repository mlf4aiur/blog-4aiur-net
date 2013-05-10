Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2012/01/binding-multiple-ip-addresses-on-the-same-network-interface/
Post: 660
Title: Binding Multiple IP Addresses on the Same Network Interface
Slug: binding-multiple-ip-addresses-on-the-same-network-interface
Postformat: standard
Keywords: MacOSX
Status: publish
Date: 2012-01-13 10:22:52 +0800
Pings: On
Comments: On
Category: MacOSX

### Binding Multiple IP Addresses on the Same Network Interface

**Add new address**

<pre lang="bash">
sudo ifconfig en1 alias 192.168.2.2 netmask 255.255.255.0
sudo ifconfig en1 alias 192.168.3.3 netmask 255.255.255.0
sudo ifconfig en1 alias 192.168.4.4 netmask 255.255.255.0
sudo ifconfig lo0 alias 127.0.0.2
</pre>

**Remove alias address**

<pre lang="bash">
sudo ifconfig en1 remove 192.168.2.2 netmask 255.255.255.0
sudo ifconfig en1 192.168.3.3 netmask 255.255.255.0 delete
sudo ifconfig en1 192.168.4.4 netmask 255.255.255.0 -alias
sudo ifconfig lo0 -alias 127.0.0.2
</pre>
