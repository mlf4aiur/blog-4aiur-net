Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/04/initialization-after-freebsd-installed/
Post: 108
Title: 安装好FreeBSD后的一些基础配置修改
Slug: initialization-after-freebsd-installed
Postformat: standard
Keywords: FreeBSD
Status: publish
Date: 2010-04-06 11:03:00 +0800
Pings: On
Comments: On
Category: BSD

**安装好FreeBSD后的一些基础配置修改**

* 配置alias

<pre lang="bash">[root@FreeBSD ~]# alias
alias cp='cp -i'
alias grep='grep --color'
alias ll='ls -lG'
alias mv='mv -i'
alias rm='rm -i'
alias vi='vim'
[root@FreeBSD ~]# </pre>

* 安装sudo
<pre lang="bash">[root@FreeBSD ~]# cd /usr/ports/security/sudo
[root@FreeBSD /usr/ports/security/sudo]# make
[root@FreeBSD /usr/ports/security/sudo]# make install
[root@FreeBSD /usr/ports/security/sudo]# make clean</pre>
* 修改man的环境变量，使用less来查看手册
<pre lang="bash">FreeBSD
    -P pager  Specify which pager to use.  By default, man uses ``more -s''.
              This option overrides the PAGER environment variable.

Linux
      -P  pager
             Specify  which  pager  to  use.   This option overrides the MANPAGER environment
             variable, which in turn overrides the PAGER  variable.   By  default,  man  uses
             /usr/bin/less -is.

[4aiur@FreeBSD ~]$ tail -1 /etc/profile
PAGER="less -is"; export PAGER</pre>
* 更新locate数据库
<pre lang="bash">[4aiur@FreeBSD ~]$ sudo /etc/periodic/weekly/310.locate
Password:
Rebuilding locate database:
＃ 让vim支持彩色
[4aiur@FreeBSD ~]$ locate vimrc_example.vim
/usr/local/share/vim/vim72/gvimrc_example.vim
/usr/local/share/vim/vim72/vimrc_example.vim
[4aiur@FreeBSD ~]$ cp -pf /usr/local/share/vim/vim72/vimrc_example.vim ~/.vim</pre>
