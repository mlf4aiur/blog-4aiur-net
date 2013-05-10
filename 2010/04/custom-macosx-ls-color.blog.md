Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/04/custom-macosx-ls-color/
Post: 121
Title: 定制MacOSX ls颜色
Slug: custom-macosx-ls-color
Postformat: standard
Keywords: MacOSX
Status: publish
Date: 2010-04-06 11:20:00 +0800
Pings: On
Comments: On
Category: BSD

**定制MacOSX ls颜色**

习惯了linux的ls颜色，不适应MacOSX终端中的ls颜色修改了ls颜色使用的默认环境变量，跟linux有些接近了。

首先增加一个

<pre lang="bash">aliasalias ll='ls -lG'</pre>

之后修改LSCOLORS环境变量

<pre lang="bash">export LSCOLORS=ExGxCxDxCxEgEdAbAgAcAd</pre>

LSCOLORS from man ls:

> LSCOLORS The value of this variable describes what color to use for which attribute when colors are enabled with CLICOLOR. This string is a concatenation of pairs of the format fb, where f is the fore- ground color and b is the background color.
> The default is "exfxcxdxbxegedabagacad", i.e. blue foreground and default background for regular directories, black foreground and red background for setuid executables, etc.

linux颜色可以细化到后缀名，这一点做的很不错，下面是Linux的ls颜色控制变量

<pre lang="bash">[root@localhost ~]# env | grep LS_COLORS
LS_COLORS=no=00:fi=00:di=01;34:ln=01;36:pi=40;33:so=01;35:bd=40;33;01:cd=40;33;01:or=01;05;37;41:mi=01;05;37;41:ex=01;32:*.cmd=01;32:*.exe=01;32:*.com=01;32:*.btm=01;32:*.bat=01;32:*.sh=01;32:*.csh=01;32:*.tar=01;31:*.tgz=01;31:*.arj=01;31:*.taz=01;31:*.lzh=01;31:*.zip=01;31:*.z=01;31:*.Z=01;31:*.gz=01;31:*.bz2=01;31:*.bz=01;31:*.tz=01;31:*.rpm=01;31:*.cpio=01;31:*.jpg=01;35:*.gif=01;35:*.bmp=01;35:*.xbm=01;35:*.xpm=01;35:*.png=01;35:*.tif=01;35:</pre>
