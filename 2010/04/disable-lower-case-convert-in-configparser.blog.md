Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/04/disable-lower-case-convert-in-configparser/
Post: 160
Title: 禁用ConfigParser配置选项转化小写
Slug: disable-lower-case-convert-in-configparser
Postformat: standard
Keywords: Python
Status: publish
Date: 2010-04-20 10:05:54 +0800
Pings: Off
Comments: On
Category: Python

**禁用ConfigParser配置选项转化小写**

ConfigParser在读取配置文件后默认会把配置项目转换为小写，调用下面的方法可以关闭转换功能。

<pre lang="bash">cfgparser.optionxform = str</pre>
