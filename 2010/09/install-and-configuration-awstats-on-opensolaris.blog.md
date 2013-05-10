Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/09/install-and-configuration-awstats-on-opensolaris/
Post: 264
Title: 在OpenSolais上的安装与配置awstats
Slug: install-and-configuration-awstats-on-opensolaris
Postformat: standard
Keywords: awstats, OpenSolaris
Status: publish
Date: 2010-09-17 10:51:15 +0800
Pings: On
Comments: On
Category: OpenSolaris

**在OpenSolais上的安装与配置awstats**

安装awstats

<pre lang="bash"># pkgin in awstats</pre>

安装GeoIP

<pre lang="bash"># pkgin in GeoIP</pre>

安装perl的GeoIP模块

<pre lang="bash"># Geo::IP::PurePerl
# wget http://geolite.maxmind.com/download/geoip/api/pureperl/Geo-IP-PurePerl-1.25.tar.gz
# gtar zxf Geo-IP-PurePerl-1.25.tar.gz
# cd Geo-IP-PurePerl-1.25
# perl Makefile.PL 
# make
# make test
# make install</pre>

下载GeoCity库

<pre lang="bash"># wget http://geolite.maxmind.com/download/geoip/database/GeoLiteCity.dat.gz
# gzip -d GeoLiteCity.dat.gz
# mv GeoLiteCity.dat /opt/local/share/GeoIP/</pre>

配置apache的虚拟主机

<pre lang="html"># cd /opt/local/etc/httpd/virtualhosts/
# vi blog.4aiur.net.conf
<VirtualHost *:80>
    ServerName blog.4aiur.net
    DocumentRoot /home/4aiur/web/blog.4aiur.net/
    DocumentRoot /home/4aiur/web/blog.4aiur.net/
    CustomLog /home/4aiur/web/logs/blog.4aiur.net_access_log combined
    ErrorLog /home/4aiur/web/logs/blog.4aiur.net_error.log
    <Directory /home/4aiur/web/blog.4aiur.net/>
        Options -Indexes IncludesNOEXEC FollowSymLinks -MultiViews
        AllowOverride All
        Order allow,deny
        Allow from all
    </Directory>
    Alias /awstatsclasses "/usr/local/awstats/classes/"
    Alias /awstatscss "/usr/local/awstats/css/"
    Alias /awstatsicons "/usr/local/awstats/icon/"
    ScriptAlias /awstats/ /usr/local/awstats/cgi-bin/
    <Directory "/usr/local/awstats/">
        Options None
        AllowOverride None
        Order allow,deny
        Allow from all
    </Directory>
</VirtualHost>
# svcadm restart apache</pre>

配置awstats配置文件

<pre lang="bash"># cd /opt/local/etc/awstats/
# mv awstats.model.conf awstats.blog.4aiur.net.conf
# vi awstats.blog.4aiur.net.conf
LogFile="/home/4aiur/web/logs/blog.4aiur.net_access_log"
SiteDomain="blog.4aiur.net"
HostAliases="localhost 127.0.0.1 REGEX[4aiur\.net$]"
DNSLookup=0
DirIcons="/awstatsicons"
SkipFiles="REGEX[^\/wp-]"
LoadPlugin="geoip GEOIP_STANDARD /opt/local/share/GeoIP/GeoIP.dat"
LoadPlugin="geoip_city_maxmind GEOIP_STANDARD /opt/local/share/GeoIP/GeoLiteCity.dat"
</pre>

增加日志分析程序到crontab

<pre lang="bash">
0 * * * * /opt/local/awstats/bin/awstats_updateall.pl now >/dev/null 2>&1</pre>

访问方式  
<http://blog.4aiur.net/awstats/awstats.pl?config=blog.4aiur.net>
