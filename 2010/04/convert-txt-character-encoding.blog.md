Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/04/convert-txt-character-encoding/
Post: 131
Title: 转换txt文件的字符编码
Slug: convert-txt-character-encoding
Postformat: standard
Keywords: shell
Status: publish
Date: 2010-04-06 11:24:47 +0800
Pings: On
Comments: On
Category: Shell

**转换txt文件的字符编码**

<pre lang="bash">find . -type f -name "*.txt" |\
while read line
do
   if iconv -f GB2312 -t UTF-8 "$line" > /tmp/foo; then
       mv /tmp/foo "$line"
   else
       :
   fi
done</pre>
