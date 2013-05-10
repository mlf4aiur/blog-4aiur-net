Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/04/parse-excel-using-python/
Post: 113
Title: 使用Python解析Excel
Slug: parse-excel-using-python
Postformat: standard
Keywords: Excel, Python
Status: publish
Date: 2010-04-06 11:05:11 +0800
Pings: On
Comments: On
Category: Python

**使用Python解析Excel**

查看pyExcelerator的tools中xls2txt.py了解到pyExcelerator使用parse_xls方法解析excel文件。
调用parse_xls方法后，生成的结果为list，list的结构示例如下：

<pre lang="bash">[(u'Sheet1', {(0, 0): 111, (0, 1): 112, (1, 0): 121, (1, 1): 122}),
(u'Sheet2', {(0, 0): 211, (0, 1): 212, (1, 0): 221, (1, 1): 222}),
(u'Sheet3', {})]</pre>

<pre lang="python">localhost:Share 4aiur$ ipython
Leopard libedit detected.
Python 2.6.1 (r261:67515, Jul  7 2009, 23:51:51)
Type "copyright", "credits" or "license" for more information.

IPython 0.10 -- An enhanced Interactive Python.
?         -> Introduction and overview of IPython's features.
%quickref -> Quick reference.
help      -> Python's own help system.
object?   -> Details about 'object'. ?object also works, ?? prints more.

In [1]: from pyExcelerator import *

In [2]: l =  parse_xls('foo.xls')

In [3]: l
Out[3]:
[(u'Sheet1', {(0, 0): 111, (0, 1): 112, (1, 0): 121, (1, 1): 122}),
(u'Sheet2', {(0, 0): 211, (0, 1): 212, (1, 0): 221, (1, 1): 222}),
(u'Sheet3', {})]

In [4]: </pre>

了解到解析的数据结构后，既可根据自己的需求进行数据分析。

<pre lang="python">ocalhost:Share 4aiur$ cat > foo.py
#!/usr/bin/env python
# -*- coding: utf-8 -*-

from pyExcelerator import *
l = parse_xls('foo.xls', 'utf-8')   # parse_xls(arg) -- default encoding
for sheet_name, values in l:
  print 'Sheet = "%s"' % sheet_name.encode('utf-8', 'backslashreplace')
  for row_idx, col_idx in sorted(values.keys()):
        v = values[(row_idx, col_idx)]
        if isinstance(v, unicode):
           v = v.encode('utf-8', 'backslashreplace')
        else:
           v = str(v)
        print '(%d, %d) =' % (row_idx, col_idx), v
localhost:Share 4aiur$ chmod +x foo.py
localhost:Share 4aiur$ ./foo.py
Sheet = "Sheet1"
(0, 0) = 111
(0, 1) = 112
(1, 0) = 121
(1, 1) = 122
Sheet = "Sheet2"
(0, 0) = 211
(0, 1) = 212
(1, 0) = 221
(1, 1) = 222
Sheet = "Sheet3"
localhost:Share 4aiur$ </pre>
