Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2011/06/install-and-use-ipmitool-on-centos/
Post: 374
Title: CentOS安装与使用ipmitool
Slug: install-and-use-ipmitool-on-centos
Postformat: standard
Keywords: ipmitool
Status: publish
Date: 2011-06-10 22:24:12 +0800
Pings: On
Comments: On
Category: SysAdmin

**CentOS安装与使用ipmitool**

ipmitoool可以方便的查看设备硬件状态，建议设备在安装好系统后安装一下ipmitool

安装方法：

安装程序包
<pre lang="bash">yum -y install OpenIPMI.x86_64 OpenIPMI-tools.x86_64 OpenIPMI-libs.x86_64 ipmitool.x86_64</pre>
添加ipmi module
<pre lang="bash">modprobe ipmi_si 
modprobe ipmi_devintf
modprobe ipmi_msghandler</pre>

查看module是否成功添加
<pre lang="bash">lsmod | grep -i ipmi
ipmi_si                77900  0 
ipmi_devintf           44688  0 
ipmi_msghandler        73176  2 ipmi_si,ipmi_devintf</pre>

查看系统事件日志命令
<pre lang="bash">ipmitool -I open sel list
   1 | 09/02/2010 | 04:10:26 | OEM #0x02 | 
   2 | 09/02/2010 | 04:10:29 | OEM #0x02 | 
   3 | 03/23/2011 | 16:02:59 | OEM #0x02 | 
   4 | 03/23/2011 | 16:03:02 | OEM #0x02 | </pre>

清理系统事件日志命令
<pre lang="bash">ipmitool -I open sel clear</pre>
