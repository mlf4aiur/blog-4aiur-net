Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/04/python2-5-4-httplib-timeout-patch/
Post: 137
Title: python2.5.4 httplib timeout patch
Slug: python2-5-4-httplib-timeout-patch
Postformat: standard
Keywords: httplib, Python
Status: publish
Date: 2010-04-06 11:28:30 +0800
Pings: On
Comments: On
Category: Python

<pre lang="bash">632c632
<     def __init__(self, host, port=None, strict=None, timeout=None):
---
>     def __init__(self, host, port=None, strict=None):
643,644d642
<         self.timeout = timeout
<
673,674d670
<                 if self.timeout is not None:
<                         self.sock.settimeout(self.timeout)</pre>
