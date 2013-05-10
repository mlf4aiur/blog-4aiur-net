Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2011/02/macosx-services-tips/
Post: 340
Title: MacOSX Services使用技巧
Slug: macosx-services-tips
Postformat: standard
Keywords: MacOSX, Services, Tips
Status: publish
Date: 2011-02-23 12:08:57 +0800
Pings: On
Comments: On
Category: MacOSX

**MacOSX Services使用技巧**

在MacOSX中利用Services快速完成某些工作，下面我举几个小例子演示一下。

1. 简繁字转换  
在文本编辑器中选中需要转换的文字后点击桌面左上角的编辑器名 -> Services -> Text -> Convert Selected Simplified Chinese Text  
<img src="http://blog.4aiur.net/wp-content/uploads/2011/02/services_convert_text.png" alt="Services Convert Text" />
2. 快速把文档当作附件发送  
在Finder中选中文件后点击桌面左上角的Finder -> Services -> Messaging -> New Email With Attachment  
<img src="http://blog.4aiur.net/wp-content/uploads/2011/02/services_new_email_with_attachment.png" alt="Services new Email With Attachment" />
3. 使用Textmate打开文件夹  
打开Automator新建Service，在Actions中搜索Open Finder Items，双击或者把“Open Finder Items”拖动到右边，在右边的Open with:中选中Textmate，保存为Open in TextMate。  
之后有需要新项目需要用TextMate打开，就可以直接掉用Services了。  
Finder -> Services -> Files and Folders -> Open in TextMate  
<img src="http://blog.4aiur.net/wp-content/uploads/2011/02/services_open_in_text_mate.png" alt="Services Open in Text Mate" />

附:  
Textmate官网提供了一个Finder中的Tools[open in textmate from finder](http://henrik.nyh.se/2007/10/open-in-textmate-from-leopard-finder)，用这个工具也挺方便的。  
安装好后在Finder中选中要打开的Folder，点击OpenInTextMate即可。
