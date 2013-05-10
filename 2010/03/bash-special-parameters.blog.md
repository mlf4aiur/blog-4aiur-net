Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/03/bash-special-parameters/
Post: 35
Title: 特殊Bash变量
Slug: bash-special-parameters
Postformat: standard
Keywords: Bash
Status: publish
Date: 2010-03-31 17:40:46 +0800
Pings: On
Comments: On
Category: Shell

<p><strong>特殊Bash变量</strong></p>
<table border="2" cellpadding="1" cellspacing="1" summary="特殊Bash变量" width="100%">
	<tbody>
		<tr>
			<th align="left" width="40">字符</th>
			<th align="left" width="922">定义</th>
		</tr>
	</tbody>
	<tbody>
		<tr>
			<td align="left">$*</td>
			<td align="left">展开为位置参数，从1开始。当扩展发生在双引号时，它展开成一个单独的词，每个参数的值由 IFS 特殊变量的第一个字符分隔。</td>
		</tr>
		<tr>
			<td align="left">$@</td>
			<td align="left">展开为位置参数，从1开始。当在双引号里展开时，每个参数展开成独立的词。</td>
		</tr>
		<tr>
			<td align="left">$#</td>
			<td align="left">把位置参数展开为十进制数字。</td>
		</tr>
		<tr>
			<td align="left">$?</td>
			<td align="left">展开成最近执行的前台管道程序的退出状态。</td>
		</tr>
		<tr>
			<td align="left">$-</td>
			<td align="left">一个连字符展开为当前选项标志 内部命令集 或者那些shell自己的集（如-i）。A hyphen expands to the current option flags as specified upon invocation, by the&nbsp;<strong>set</strong> built-in command, or those set by the shell itself (such as the -i).</td>
		</tr>
		<tr>
			<td align="left">$</td>
			<td align="left">展开成shell的进程ID。</td>
		</tr>
		<tr>
			<td align="left">$!</td>
			<td align="left">展开成最近在后台（异步）执行的命令的进程ID。</td>
		</tr>
		<tr>
			<td align="left">$0</td>
			<td align="left">展开成shell或者shell脚本名。</td>
		</tr>
		<tr>
			<td align="left">$_</td>
			<td align="left">下划线变量在shell启动时设置，包含shell的绝对文件名或者作为参数列表被执行的脚本。随后，它展开为前一个命令扩展后的最后一个参数。它同样设置为每个执行程序的全路径，放在那个命令的输出环境中。当检查邮件时，这个参数保存邮件文件的名字。The underscore variable is set at shell startup and contains the absolute file name of the shell or script being executed as passed in the argument list. Subsequently, it expands to the last argument to the previous command, after expansion. It is also set to the full pathname of each command executed and placed in the environment exported to that command.</td>
		</tr>
	</tbody>
</table>
