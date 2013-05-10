Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2011/01/install-getbundles/
Post: 318
Title: Install GetBundles
Slug: install-getbundles
Postformat: standard
Keywords: Bundle, TextMate
Status: publish
Date: 2011-01-10 09:13:55 +0800
Pings: On
Comments: On
Category: MacOSX

**Install GetBundles**

管理TextMate Bundle的工具，方便查找、安装、更新TextMate Bundle的工具。

<pre lang="bash">mkdir -p ~/Library/Application\ Support/TextMate/Bundles
cd !$
svn co http://svn.textmate.org/trunk/Review/Bundles/GetBundles.tmbundle/
osascript -e 'tell app "TextMate" to reload bundles'</pre>
