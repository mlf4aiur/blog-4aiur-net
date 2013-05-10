Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/03/statistics-squid-cache-files/
Post: 76
Title: 统计squid下分频道cache文件总量
Slug: statistics-squid-cache-files
Postformat: standard
Keywords: awk, Squid
Status: publish
Date: 2010-03-31 21:33:01 +0800
Pings: On
Comments: On
Category: SysAdmin

**统计squid下分频道cache文件总量**

今天有个朋友问我怎么查看squid下分频道的cache文件总量

想了想用下面的命令实现起来方便又快捷，^_^

<pre lang="bash">strings swap.state | \
    awk -F"/" /^http/'{if ($3 in host) host[$3]++; else host[$3]=1}END{for (item in host) print item,host[item] | "sort -k2 -nr"}'</pre>

同一个url是否压缩和编码的区别的话，会计算成多个。
