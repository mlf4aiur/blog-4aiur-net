Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/06/run-remote-complex-command-without-backslash-over-ssh/
Post: 173
Title: 不需要反引号的运行ssh复杂远程命令方式
Slug: run-remote-complex-command-without-backslash-over-ssh
Postformat: standard
Keywords: shell, ssh
Status: publish
Date: 2010-06-07 15:07:22 +0800
Pings: Off
Comments: On
Category: Shell

**不需要反引号的运行ssh复杂远程命令方式**

之前当我需要执行复杂的运程命令时，总是要先处理好引号、变量等问题，之后再执行命令写起来太麻烦，今天在commandlinefu上学了一招非常棒的方法，记录一下。

举个例子，之前需要用\反引$符号

<pre lang="bash">[root@localhost ~]# ssh 127.0.0.1 -l root "echo a b c | awk '{print \$2}'"
b</pre>

现在我们可以把复杂的命令写到文件中执行。

<pre lang="bash">[root@localhost ~]# cat cmd
echo a b c | awk '{print $2}'</pre>

方法1:

<pre lang="bash">[root@localhost ~]# ssh 127.0.0.1 -l root "$(<cmd)"
b</pre>

方法2:
<pre lang="bash">[root@localhost ~]# ssh 127.0.0.1 -l root "`cat cmd`"
b</pre>

方法3，使用标准输入执行，输入完毕后使用ctrl-D提交命令:
<pre lang="bash">[root@localhost ~]# ssh 127.0.0.1 -l root "`cat -`"
hostname
if [ -d /root/ ];then
  echo "ok"
else
  echo "not ok"
fi
ctrl-D
localhost
ok
[root@localhost ~]#</pre>

来源：
http://www.commandlinefu.com/commands/view/5772/run-complex-remote-shell-cmds-over-ssh-without-escaping-quotes

<pre lang="bash">ssh host -l user $(<cmd.txt)</pre>
run complex remote shell cmds over ssh, without escaping quotes
Much simpler method. More portable version:

<pre lang="bash">ssh host -l user "`cat cmd.txt`"</pre>

<pre lang="bash">perl -e 'system @ARGV, <stdin>' ssh host -l user < cmd.txt</pre>

run complex remote shell cmds over ssh, without escaping quotes

I was tired of the endless quoting, unquoting, re-quoting, and escaping characters that left me with working, but barely comprehensible shell one-liners. It can be really frustrating, especially if the local and remote shells differ and have their own escaping and quoting rules. I decided to try a different approach and ended up with this.

so we need to save the command in cmd.txt first and then run it?

there's no way to do this? :
<pre lang="bash">ssh host -l user $(perl -ane "print $F[1]\n" filename )</pre>

one alternative I can think of (not to save a file and then use "cat cmd.txt") is;
<pre lang="bash">ssh host -l user "`cat -`"</pre>

And then type your command, press Enter and then ctrl-D. This will allow you run complex commands on remote machine without saving a file..
