Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/03/remove-apache-illegal-log/
Post: 70
Title: 排除Apache access log乱序日志
Slug: remove-apache-illegal-log
Postformat: standard
Keywords: awk, shell
Status: publish
Date: 2010-03-31 21:28:30 +0800
Pings: On
Comments: On
Category: Shell

**排除Apache access log乱序日志**

由于Apache的访问日志时间记录的是访问开始时间，所以会有时间不是顺序排列的情况产生。  
由于有一个特殊需求，需要把乱序的日志排除掉，今天写了个小脚本处理了一下。  
转换Apache accesslog时间为时间戳，进行处理  
把乱序日志打印到了badlog文件中  

<pre lang="bash">[root@4Aiur ~]# cat foo
58.59.23.18 - - [26/Nov/2008:11:04:05 +0800] "GET /test.html HTTP/1.1" 200 8228 "-" "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 1.1.4322; .NET CLR 2.0.50727)"
58.59.23.18 - - [26/Nov/2008:11:03:05 +0800] "GET /test.html HTTP/1.1" 200 8228 "-" "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 1.1.4322; .NET CLR 2.0.50727)"
58.59.23.18 - - [26/Nov/2008:11:05:05 +0800] "GET /test.html HTTP/1.1" 200 8228 "-" "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 1.1.4322; .NET CLR 2.0.50727)"

[root@4Aiur ~]# cat foo.awk
#!/bin/awk

function date2unixstamp(date) {
   sub(/\[/,"",date)
   split(date,array,/[/:]/)
   if (array[2]=="Nov") array[2]="11"
   else if (array[2]=="Jan") array[2]="01"
   else if (array[2]=="Feb") array[2]="02"
   else if (array[2]=="Mar") array[2]="03"
   else if (array[2]=="Apr") array[2]="04"
   else if (array[2]=="May") array[2]="05"
   else if (array[2]=="Jun") array[2]="06"
   else if (array[2]=="Jul") array[2]="07"
   else if (array[2]=="Aug") array[2]="08"
   else if (array[2]=="Sep") array[2]="09"
   else if (array[2]=="Oct") array[2]="10"
   #else if (array[2]=="Nov") array[2]="11"
   else if (array[2]=="Dec") array[2]="12"
   Time=array[3]" "array[2]" "array[1]" "array[4]" "array[5]" "array[6]" "array[7]
   Timestamp=mktime(Time)
   return Timestamp
}

{
   date=$4
   date2unixstamp(date)
   #print date,Timestamp,temp,Time
   if (Timestamp<temp)
       print $0 > "badlog"
   else {
       print $0 > "accesslog"
       temp=Timestamp
   }
}

[root@4Aiur ~]# awk -f foo.awk foo
[root@4Aiur ~]# cat accesslog
58.59.23.18 - - [26/Nov/2008:11:04:05 +0800] "GET /test.html HTTP/1.1" 200 8228 "-" "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 1.1.4322; .NET CLR 2.0.50727)"
58.59.23.18 - - [26/Nov/2008:11:05:05 +0800] "GET /test.html HTTP/1.1" 200 8228 "-" "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 1.1.4322; .NET CLR 2.0.50727)"
[root@4Aiur ~]# cat badlog
58.59.23.18 - - [26/Nov/2008:11:03:05 +0800] "GET /test.html HTTP/1.1" 200 8228 "-" "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 1.1.4322; .NET CLR 2.0.50727)"</pre>
