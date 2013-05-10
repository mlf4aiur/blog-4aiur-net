Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2011/04/upgrade-wordpress-manually/
Post: 365
Title: 手动升级wordpress的方法
Slug: upgrade-wordpress-manually
Postformat: standard
Keywords: wordpress
Status: publish
Date: 2011-04-06 15:20:39 +0800
Pings: On
Comments: On
Category: Default

**手动升级wordpress的方法**

进入到wordpress的后台管理发现有新版本更新时，使用系统自带的自动更新，总是出现300秒超时导致升级失败。  

> Downloading update from http://wordpress.org/wordpress-3.1.1.zip…  
> Download failed.: Operation timed out after 300 seconds with 1340586 bytes received  
> Installation Failed

使用terminal登陆到系统使用wget测试下载速度，发现服务器下载wordpress包的速度其慢无比，每秒只有3KB左右的速度。

<pre lang="bash">
# wget http://wordpress.org/wordpress-3.1.1.zip
--2011-04-06 15:01:31--  http://wordpress.org/wordpress-3.1.1.zip
Resolving wordpress.org... 72.233.56.138, 72.233.56.139
Connecting to wordpress.org|72.233.56.138|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: unspecified [application/zip]
Saving to: `wordpress-3.1.1.zip'
    [      <=>                                  ] 17,114      3.70K/s</pre>

解决方法：

首先进入webserver的跟目录，然后手工下载wordpress的安装包，之后修改/etc/hosts的内容把wordpress.org的地址指到本地。

<pre lang="bash">
# wget http://wordpress.org/wordpress-3.1.1.zip
# vi /etc/hosts
127.0.0.1 wordpress.org</pre>

之后再使用wordpress的自动升级，就可以成功了^_^。

> Downloading update from http://wordpress.org/wordpress-3.1.1.zip…  
> Unpacking the update…  
> Verifying the unpacked files…  
> Installing the latest version…  
> Upgrading database…  
> WordPress updated successfully  
> Go to Dashboard

升级成功后再把/etc/hosts再修改回来。
