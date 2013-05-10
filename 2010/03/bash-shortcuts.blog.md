Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/03/bash-shortcuts/
Post: 18
Title: Bash命令行编辑的快捷键
Slug: bash-shortcuts
Postformat: standard
Keywords: Bash
Status: publish
Date: 2010-03-31 16:20:18 +0800
Pings: On
Comments: On
Category: SysAdmin

**Bash命令行编辑的快捷键**

<pre>history 显示命令历史列表
↑ 显示上一条命令
↓ 显示下一条命令
!num 执行命令历史列表的第num条命令
!! 执行上一条命令
!ls 执行最后一个以ls开头的命令
Ctrl+r 然后输入若干字符，开始向上搜索包含该字符的命令，继续按Ctrl+r，搜索上一条匹配的命令
ls !$ 执行命令ls，并以上一条命令的参数为其参数
Ctrl+a 移动到当前行的开头
Ctrl+e 移动到当前行的结尾
Esc+b 移动到当前单词的开头
Esc+f 移动到当前单词的结尾
Ctrl+l 清屏
Ctrl+u 删除命令行中光标所在处之前的所有字符（不包括自身）
Ctrl+k 删除命令行中光标所在处之后的所有字符（包括自身）
Ctrl+d 删除光标所在处字符
Ctrl+h 删除光标所在处前一个字符
Ctrl+y 粘贴刚才所删除的字符
Ctrl+w 删除光标所在处之前的字符至其单词头（以空格、标点等为分隔符）
Esc+w 删除光标所在处之前的字符至其单词尾（以空格、标点等为分隔符）
Ctrl+t 颠倒光标所在处及其之前的字符位置，并将光标移动到下一个字符
Esc+t 颠倒光标所在处及其相邻单词的位置
Ctrl+(x u) 按住Ctrl的同时再先后按x和u，撤销刚才的操作
Ctrl+s 挂起当前shell
Ctrl+q 重新启用挂起的shell
Ctrl+- redo</pre>

输入set -o emacs为使用emacs（默认），输入set -o vi为使用vi为编辑器。

我们经常用的是emacs的方式控制命令行，下面是常用快捷键的解释。

<pre>Ctrl-P Move up history file.
Ctrl-N Move down history file.
Ctrl-B Move backward one character.
Ctrl-R Search backward for string.
Esc B Move backward one word.
Ctrl-F Move forward one character.
Esc F Move forward one word.
Ctrl-A Move to the beginning of the line.
Ctrl-E Move to the end of the line.
Esc < Move to the first line of the history file.
Esc > Move to the last line of the history file.

Editing with emacs: Ctrl-U Delete the line.
Ctrl-Y Put the line back.
Ctrl-K Delete from cursor to the end line.
Ctrl-D Delete a letter.
Esc D Delete one word forward.
Esc H Delete one word backward.
Esc space Set a mark at cursor position. Ctrl-X Ctrl-X Exchange cursor and mark.
Ctrl-P Ctrl-Y Push region from cursor to mark into a buffer (Ctrl-P) and put it down (Ctrl-Y).</pre>
