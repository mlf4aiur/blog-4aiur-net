Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/03/rsync-match-mode/
Post: 74
Title: rsync模式匹配
Slug: rsync-match-mode
Postformat: standard
Keywords: rsync
Status: publish
Date: 2010-03-31 21:32:04 +0800
Pings: On
Comments: On
Category: SysAdmin

**rsync模式匹配**

今天有一个同步数据的小问题，需要把一些符合特定日期的文件保存到另外一个目录，使用shell也很容易实现，之前没用rsync做过，今天顺便研究了一下rsync的实现方式，rsync是使用排除和取消排除的方法（诡异）。

为了实现递归，先写一个不排除的规则--include="*/" 

再写一个希望保存文件的规则--include="*2009022[78]*" 

最后写上排除所有的规则--exclude="*"

组合以上3个选项实现了对特定文件的同步。

<pre lang="bash">rsync -aruv --include="*/" --include="*2009022[78]*" --include="*2009030[12]*" --exclude="*" /source /destination

# aruv比较常用

-a, --archive               archive mode, equivalent to -rlptgoD

-r, --recursive             recurse into directories

-u, --update                update only (don't overwrite newer files)

-v, --verbose               increase verbosity</pre>
