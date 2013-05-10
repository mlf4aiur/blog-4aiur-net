Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/10/zabbix-install-manual/
Post: 282
Title: zabbix简易安装手册
Slug: zabbix-install-manual
Postformat: standard
Keywords: Zabbix
Status: publish
Date: 2010-10-15 16:50:00 +0800
Pings: On
Comments: On
Category: SysAdmin

**zabbix简易安装手册**

1) 安装zabbix依赖包

<pre lang="bash"># yum install gcc MySQL-python mysql mysql-server mysql-devel mysql-bench php php-mysql php-bcmath php-mbstring freetype php-gd php-xml curl-devel libpurple yum install net-snmp net-snmp-devel

# mkdir package
# cd package/
# wget http://packages.sw.be/iksemel/iksemel-devel-1.4-1.el5.rf.x86_64.rpm
# wget http://packages.sw.be/iksemel/iksemel-1.4-1.el5.rf.x86_64.rpm
# rpm -ivh iksemel-1.4-1.el5.rf.x86_64.rpm iksemel-devel-1.4-1.el5.rf.x86_64.rpm
# wget http://fping.sourceforge.net/download/fping.tar.gz
# tar zxf fping.tar.gz
# cd fping-2.4b2_to/
# ./configure && make && make install</pre>
2) 配置zabbix依赖环境

<pre lang="bash"># service httpd restart
# chkconfig httpd on
# chkconfig mysqld on
# service mysqld restart</pre>

3) 在mysql中增加zabbix库并配置zabbix库访问权限

<pre lang="bash"># mysql
mysql> create database zabbix character set utf8;
mysql> GRANT ALL ON zabbix.* TO 'zabbix'@'localhost' IDENTIFIED BY 'zabbix';
mysql> quit;</pre>

4) 下载并安装zabbix

<pre lang="bash"># wget "http://prdownloads.sourceforge.net/zabbix/zabbix-1.8.3.tar.gz?download"
# tar zxf zabbix-1.8.3.tar.gz 
# cd zabbix-1.8.3
# cd create/schema
# cat mysql.sql | mysql -uroot zabbix
# cd ../data
# cat data.sql | mysql -uroot zabbix
# cat images_mysql.sql | mysql -uroot zabbix
# cd ../../
# ./configure  --enable-server --enable-agent --with-mysql --with-net-snmp --with-jabber --with-libcurl
# enable Jabber 即时通讯功能。Jabber 是著名的Linux即时通讯服务服务器，它是一个自由开源软件，能让用户自己架即时通讯服务器，可以在Internet上应用，也可以在局域网中应用。 Jabber最有优势的就是其通信协议，可以和多种即时通讯对接。比如有第三方插件，能让jabber用户和MSN 、Yahoo、ICQ等IM用户相互通讯。 
# make install</pre>

5) 简单配置zabbix

<pre lang="bash"># vim /etc/services
    zabbix-agent    10050/tcp  Zabbix Agent
    zabbix-agent    10050/udp  Zabbix Agent
    zabbix-trapper  10051/tcp  Zabbix Trapper
    zabbix-trapper  10051/udp  Zabbix Trapper 
# mkdir /etc/zabbix/
# cp /root/package/zabbix-1.8.3/misc/conf/zabbix_server.conf /etc/zabbix/zabbix_server.conf
# vim /etc/zabbix/zabbix_server.conf
    LogFile=/var/log/zabbix/zabbix_server.log
    LogFileSize=100
    DebugLevel=3
    PidFile=/tmp/zabbix_server.pid
    DBHost=localhost
    DBName=zabbix
    DBPassword=zabbix
    DBSocket=/var/lib/mysql/mysql.sock
    DBPort=3306
    ListenIP=0.0.0.0
    StartPingers=30
    FpingLocation=/usr/local/sbin/fping

# cp /root/package/zabbix-1.8.3/misc/conf/zabbix_agent.conf /etc/zabbix/
# cp /root/package/zabbix-1.8.3/misc/conf/zabbix_agentd.conf /etc/zabbix/zabbix_agentd.conf
# mv frontends/php /var/www/html/zabbix

# vim /etc/php.ini
    max_execution_time = 600     ; Maximum execution time of each script, in seconds
    max_input_time = 600    ; Maximum amount of time each script may spend parsing request data
    memory_limit = 256M      ; Maximum amount of memory a script may consume
    post_max_size = 32M
    upload_max_filesize = 16M
    date.timezone = Asia/Shanghai
# service httpd restart
# useradd -s /sbin/nologin zabbix</pre>

6) 启动zabbix

<pre lang="bash"># su zabbix -c /usr/local/sbin/zabbix_server
# su zabbix -c /usr/local/sbin/zabbix_agentd</pre>
使用http://IP/zabbix登录zabbix，首次登录时按照网页提示进行操作。
默认登录帐户与密码：
admin/zabbix
