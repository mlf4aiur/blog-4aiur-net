Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2011/03/install-and-configuration-snmpd-on-linux/
Post: 345
Title: Linux安装与配置Snmpd
Slug: install-and-configuration-snmpd-on-linux
Postformat: standard
Keywords: Linux, snmpd
Status: publish
Date: 2011-03-16 11:02:59 +0800
Pings: On
Comments: On
Category: Linux

Linux安装与配置Snmpd

下面是两个主流的Linux，CentOS与Ubuntu的snmpd安装与自动配置脚本

Script on CentOS

<pre lang="bash">#!/usr/bin/bash
# Install snmp and agent
yum install -y net-snmp net-snmp-utils

# Backup snmpd.conf
mv /etc/snmp/snmpd.conf /etc/snmp/snmpd.conf-`date +%Y%m%d%H%M%S`

# Write new snmpd config
cat > /etc/snmp/snmpd.conf <<\EOF
com2sec notConfigUser  default       public
group   notConfigGroup v2c           notConfigUser
view    systemview    included   .1.3.6.1.2.1.2.2
view    systemview    included   .1.3.6.1.2.1.25.1.1
view    systemview    included   .1.3.6.1.4.1.2021
access  notConfigGroup ""      any       noauth    exact  systemview    none   none
syslocation Unknown (edit /etc/snmp/snmpd.conf)
syscontact Root <root@localhost> (configure /etc/snmp/snmp.local.conf)
disk / 20%
EOF

chkconfig snmpd on
/etc/init.d/snmpd restart</pre>

Script on Unbuntu

<pre lang="bash">#!/usr/bin/bash
# Install snmp and agent
apt-get -y install snmpd snmp

# Modify snmpd listening ipaddress
sed -i 's/127.0.0.1/0.0.0.0/g' /etc/default/snmpd

# Backup snmpd.conf
mv /etc/snmp/snmpd.conf /etc/snmp/snmpd.conf-`date +%Y%m%d%H%M%S`

# Write new snmpd config
cat > /etc/snmp/snmpd.conf <<\EOF
com2sec notConfigUser  default       public
group   notConfigGroup v2c           notConfigUser
view    systemview    included   .1.3.6.1.2.1.2.2
view    systemview    included   .1.3.6.1.2.1.25.1.1
view    systemview    included   .1.3.6.1.4.1.2021
access  notConfigGroup ""      any       noauth    exact  systemview    none   none
syslocation Unknown (edit /etc/snmp/snmpd.conf)
syscontact Root <root@localhost> (configure /etc/snmp/snmp.local.conf)
disk / 20%
EOF
/etc/init.d/snmpd restart</pre>
