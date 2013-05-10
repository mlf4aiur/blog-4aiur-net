Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/03/using-ssh-to-automation/
Post: 23
Title: 使用ssh实现系统管理的自动化
Slug: using-ssh-to-automation
Postformat: standard
Keywords: shell, ssh
Status: publish
Date: 2010-03-31 16:23:39 +0800
Pings: On
Comments: On
Category: SysAdmin

**使用ssh实现系统管理的自动化**

常见的几种ssh认证方式：
1、密码认证
2、使用明文密钥
3、使用加密密钥
从安全角度考虑，建议使用第三种方式“使用加密密钥”实施自动化。

一、密码认证的自动化
使用伪终端与SSH进行交互(例如,使用类似expect的工具)
一个更改密码的小例子

<pre lang="bash">#!/bin/sh
#
#
# SCRIPT: chpass
#
# AUTHOR: Kevin Lee
#
# DATE: 05-17-2006
#
# PURPOSE:  This script is change nodes password.
#
#
# REV: 1.0.A
#
# REV.LIST:
#
#
# set -x # Uncomment to debug this script
# set -n # Uncomment to check command syntax without any execution
#
#######################################################

# Define and initialize variables here...

INPUTFILE="filelist"
NEWPASSWD="newpassword"
rm -f ~/.ssh/known_hosts

##################################################
############ START of MAIN #######################
##################################################

while read HOSTNAME OLDPASSWD IPADDRESS
do
LOG="logs/tmp_$HOSTNAME_.log"
EXP="tmp_$HOSTNAME_.exp"
SH="tmp_$HOSTNAME_.sh"
echo spawn ssh -l root $IPADDRESS >$EXP
echo 'expect "*(yes/no)?"' >>$EXP
echo send \"yes\\r\" >>$EXP
echo 'expect "*assword:"' >>$EXP
echo send \"$OLDPASSWD\\r\" >>$EXP
echo 'expect "#"' >>$EXP
echo 'send "passwd\r"' >>$EXP
echo expect \"*New password:\" >>$EXP
echo send \"$NEWPASSWD\\r\" >>$EXP
echo expect \"*new password:\" >>$EXP
echo send \"$NEWPASSWD\\r\" >>$EXP
echo expect '"*#" {send "exit\r"}' >>$EXP
echo expect eof  >>$EXP
echo echo $HOSTNAME $IPADDRESS > $SH
echo expect $EXP >>$SH
echo echo DONE >>$SH
sh $SH > $LOG &
done < $INPUTFILE
rm -f tmp_*.exp
rm -f tmp_*.sh</pre>

二、使用明文密钥

不推荐使用明文密钥,因为你把私钥放在服务器上执行自动化操作,任何可以读取该私钥的帐号都可以对你的服务器做出破坏.

三、使用加密密钥

强烈推荐使用此方法.  
通过使用ssh-agent把密钥加载到内存中,在进行批处理的时候加载一次私钥,使用完毕后卸载私钥.  
下面是一个小例子,供大家参考,希望大家可以发挥想象,做更多的事情.

<pre lang="bash">#!/bin/sh
#set -x
INPUTFILE="filelist"

while read HOSTNAME IPADDRESS
do
LOG="UPDIR/tmp_$HOSTNAME.log"
SH="tmp_$HOSTNAME.sh"
echo echo $SN $HOSTNAME $IPADDRESS > $SH
echo "echo" >> $SH
echo scp tempfile $IPADDRESS:/tmp/tempfile >> $SH
echo ssh $IPADDRESS "ls -l /tmp/tempfile" >> $SH
echo "echo" >> $SH
sh $SH > $LOG &
done < $INPUTFILE
sleep 1
rm -f tmp*.sh</pre>
