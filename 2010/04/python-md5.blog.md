Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/04/python-md5/
Post: 90
Title: 使用python计算md5
Slug: python-md5
Postformat: standard
Keywords: Python
Status: publish
Date: 2010-04-06 10:55:53 +0800
Pings: On
Comments: On
Category: Python

<pre lang="python">#!/usr/bin/env python3

import os
import hashlib

# Use hashlib module
file = open(&#39;foo&#39;, &#39;rb&#39;)
content = file.read()
file.close()
result = hashlib.md5()
result.update(content)
md5sum = result.hexdigest()
print(md5sum)

# Use OS command
result = os.popen(&#39;md5sum foo&#39;).readline()
md5sum = result.split()[0]
print(md5sum)</pre>
<br />
