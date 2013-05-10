Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/04/awk-examples/
Post: 88
Title: awk应用举例
Slug: awk-examples
Postformat: standard
Keywords: awk, shell
Status: publish
Date: 2010-04-06 10:54:56 +0800
Pings: On
Comments: On
Category: Shell

打印磁盘INODE最大值
<pre lang="bash"># df -Pi | awk 'BEGIN{OFS="\n";Max=0} {if (NR!=1) {sub(/%/,"",$5); Max=$5>Max?$5:Max}} END{print Max}'
38</pre>

打印磁盘空间最大值
<pre lang="bash"># df -P | awk 'BEGIN{OFS="\n";Max=0} {if (NR!=1) {sub(/%/,"",$5); Max=$5>Max?$5:Max}} END{print "diskUsedSpacePercent: "Max,"diskSpaceUpdateTime: "systime()}'
diskUsedSpacePercent: 55
diskSpaceUpdateTime: 1252898042</pre>

按netstat中ESTABLISHED状态的连接数量进行排序
<pre lang="bash"># netstat -an 2>/dev/null | awk /ESTABLISHED/'{print $5}' | awk -F: '{!ip[$(NF-1)]++} END { for (item in ip) print item,ip[item] | "sort -k2 -rn"}' | more</pre>

打印系统连接数
<pre lang="bash"># echo -e "\\033[1;32m"`awk /Tcp/'{print $10}' /proc/net/snmp` "\\033[0;39m"
CurrEstab 31</pre>
