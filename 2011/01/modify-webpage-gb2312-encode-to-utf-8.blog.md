Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2011/01/modify-webpage-gb2312-encode-to-utf-8/
Post: 324
Title: 修改gb2312网页编码为utf-8
Slug: modify-webpage-gb2312-encode-to-utf-8
Postformat: standard
Keywords: shell
Status: publish
Date: 2011-01-10 11:20:34 +0800
Pings: On
Comments: On
Category: Shell

**修改gb2312网页编码为utf-8**

<pre lang="bash">find html/ -type f -name "*.html" | \
while read line
do
    mkdir -p foo/`dirname $line`/
    iconv -f GB18030 -t UTF-8 $line 2>/dev/null | \
    sed /charset=gb2312/'s/charset=gb2312/charset=utf-8/' \
    > foo/$line
done</pre>
