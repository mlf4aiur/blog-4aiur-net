Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2011/12/suppressing-paramiko-log/
Post: 397
Title: Suppressing paramiko log
Slug: suppressing-paramiko-log
Postformat: standard
Keywords: logging, paramiko, Python
Status: publish
Date: 2011-12-26 11:24:13 +0800
Pings: On
Comments: On
Category: Python

**Suppressing paramiko log**

**Backup paramiko source file.**  
<pre lang="bash">cd /usr/local/lib/python2.7/site-packages/
cp -p paramiko/util.py{,.bak}</pre>

**Modify paramiko/util.py.**  
<pre lang="bash">diff paramiko/util.py{,.bak}
    
265c265
<         return not (record.name == 'suppress' and record.levelname == 'INFO')
---
>         return True</pre>

**Set logger name "suppress" in your code.**  
<pre lang="bash">self.client = SSHClient()
self.client.set_log_channel('suppress')</pre>
