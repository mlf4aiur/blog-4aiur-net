Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/03/distribute-ssh-key-using-expect/
Post: 33
Title: 使用expect配合ssh的key认证实现多台服务器的自动化处理
Slug: distribute-ssh-key-using-expect
Postformat: standard
Keywords: expect
Status: publish
Date: 2010-03-31 17:37:17 +0800
Pings: On
Comments: On
Category: SysAdmin

**使用expect配合ssh的key认证实现多台服务器的自动化处理**

使用以下方法可以方便、快速的实现多台服务器(500+)的管理，并且对中央管理服务器的配置要求不高。

**使用crontab自动更新配置.**

<pre lang="bash">[root@4Aiur ~]# crontab -l
0 * * * * /usr/sbin/ntpdate -u -t 5 cn.pool.ntp.org >/var/log/ntpsync.log 2>&1
0 23 * * * /Application/Update/run.sh >/dev/null 2>&1</pre>

**把加载key加入到系统自启动.**

[root@4Aiur ~]# cat /etc/rc.local

<pre lang="bash">#!/bin/sh
#
/root/.Batch/agent.exp</pre>

**加载key的expect脚本.**

[root@4Aiur ~]# cat /root/.Batch/agent.exp

<pre lang="bash">#!/usr/bin/expect
spawn $env(SHELL)
send "cd /root/.Batch/\r"
expect "*"
send "killall ssh-agent\r"
expect "*"
send "ssh-agent | head -2 &gt; /root/.Batch/.agent.env\r"
expect "*"
send ". /root/.Batch/.agent.env\r"
expect "*"
send "ssh-add key\r"
expect "Enter passphrase for key: "
send "example\r"
expect eof</pre>

**定时在23点的自动更新程序.**

[root@4Aiur ~]# cat /Application/Update/run.sh

<pre lang="bash">#/bin/sh
# set -x
. /root/.Batch/.agent.env
 
DEVICE_LIST="/root/4Aiur/iplist"
SH_PATH="shell"
LOG_PATH="logs"
 
cd /Application/Update
/usr/java/jdk1.5.0_06/bin/java -jar Config.jar 2>/dev/null
 
mkdir -p ${SH_PATH} ${LOG_PATH}
rm -f ${SH_PATH}/* ${LOG_PATH}/*
 
while read SN HOSTNAME IPADDRESS
do
 
(SH="${SH_PATH}/${HOSTNAME}"
LOG="${LOG_PATH}/${HOSTNAME}"
cat > ${SH} <
echo ${SN} ${HOSTNAME} ${IPADDRESS}
scp -p conf/config.ini ${IPADDRESS}:/Application//conf/config.ini
ssh ${IPADDRESS} 'md5sum /Application//conf/config.ini'
EOF
chmod +x ${SH}
./${SH} > ${LOG} 2>&1 &
)
done < ${DEVICE_LIST}
 
echo "$(date) done"</pre>
