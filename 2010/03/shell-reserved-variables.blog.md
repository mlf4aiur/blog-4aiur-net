Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/03/shell-reserved-variables/
Post: 53
Title: Shell保留变量
Slug: shell-reserved-variables
Postformat: standard
Keywords: Bash
Status: publish
Date: 2010-03-31 21:10:21 +0800
Pings: On
Comments: On
Category: Shell

<h4>保留变量</h4>
<p>Bourne shell保留变量 Bash和 Bourne shell以同一种方法来使用特定的shell变量。某些情况下，Bash为变量分配一个默认的值。下表给出一个简单的shell变量的概览：&nbsp;</p>
<h4><br />
	保留的 Bourne shell 变量</h4>
<table border="1" cellpadding="1" cellspacing="0" style="font-family: Tahoma, Arial; color: rgb(0, 0, 0); font-size: 12px; " width="100%">
	<tbody>
		<tr>
			<th align="left" width="110">变量名字</th>
			<th align="left" width="839">定义</th>
		</tr>
	</tbody>
	<tbody>
		<tr>
			<td align="left" style="word-break: break-all; ">CDPATH</td>
			<td align="left" style="word-break: break-all; ">一个由冒号分割的目录列表作为内建命令&nbsp;<b>cd</b>&nbsp;的搜索路径。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">HOME</td>
			<td align="left" style="word-break: break-all; ">当前用户的home目录；默认为内建命令&nbsp;<b>cd</b>&nbsp;。这个变量的值同样被~扩展使用。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">IFS</td>
			<td align="left" style="word-break: break-all; ">分割域的一个字符的列表；用于shell把词分开作为扩展。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">MAIL</td>
			<td align="left" style="word-break: break-all; ">如果这个变量设成一个文件名并且 MAILPATH 变量没有设置，Bash在指定文件中通知用户邮件的到达。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">MAILPATH</td>
			<td align="left" style="word-break: break-all; ">一个用冒号分隔的文件名列表，shell周期性地从这个文件里检测新邮件。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">OPTARG</td>
			<td align="left" style="word-break: break-all; "><b>getopts</b>&nbsp;内建命令处理的最后的选项参数的值。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">OPTIND</td>
			<td align="left" style="word-break: break-all; ">最后一个由&nbsp;<b>getopts</b>&nbsp;内建命令处理的选项参数的索引号。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">PATH</td>
			<td align="left" style="word-break: break-all; ">一个用冒号分隔的目录列表，shell从这些目录里寻找命令。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">PS1</td>
			<td align="left" style="word-break: break-all; ">主要提示符。默认值是 &ldquo;&#39;\s-\v\$ &#39;&rdquo;。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">PS2</td>
			<td align="left" style="word-break: break-all; ">次要提示符。默认值是 &ldquo;&#39;&gt; &#39;&rdquo;。</td>
		</tr>
	</tbody>
</table>
<h4>Bash保留变量</h4>
<div>这些变量是设置好的或者被Bash使用的，但是其他shell通常不会对它们进行特殊处理。</div>
<div><b>保留Bash变量</b></div>
<table border="1" cellpadding="0" cellspacing="0" style="font-family: Tahoma, Arial; color: rgb(0, 0, 0); font-size: 12px; " width="100%">
	<tbody>
		<tr>
			<th align="left" width="12%">变量名</th>
			<th align="left" width="88%">定义</th>
		</tr>
	</tbody>
	<tbody>
		<tr>
			<td align="left" style="word-break: break-all; ">auto_resume</td>
			<td align="left" style="word-break: break-all; ">这个变量控制shell如何与用户交互和作业控制。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">BASH</td>
			<td align="left" style="word-break: break-all; ">用于执行当前Bash实例的全路径。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">BASH_ENV</td>
			<td align="left" style="word-break: break-all; ">如果这个变量在Bash调用执行一个shell脚本时已被设置，它的值将被展开并用作在执行脚本前读取的启动文件名。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">BASH_VERSION</td>
			<td align="left" style="word-break: break-all; ">当前Bash实例的版本号。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">BASH_VERSINFO</td>
			<td align="left" style="word-break: break-all; ">一个只读变量数组，它的成员保存这个Bash实例的版本信息。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">COLUMNS</td>
			<td align="left" style="word-break: break-all; "><b>select</b>&nbsp;内建命令来决定打印选择列表时终端宽度。在收到&nbsp;<i>SIGWINCH</i>&nbsp;信号时自动设置。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">COMP_CWORD</td>
			<td align="left" style="word-break: break-all; ">包含当前光标位置的字的 ${COMP_WORDS} 的一个索引。An index into ${COMP_WORDS} of the word containing the current cursor position.</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">COMP_LINE</td>
			<td align="left" style="word-break: break-all; ">当前命令行。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">COMP_POINT</td>
			<td align="left" style="word-break: break-all; ">指明相对于当前命令起点的当前光标位置。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">COMP_WORDS</td>
			<td align="left" style="word-break: break-all; ">一个由当前命令行中单个词组成的变量数组。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">COMPREPLY</td>
			<td align="left" style="word-break: break-all; ">一个变量数组，Bash从中读取由一个可编程完整设备调用的一个shell函数生成的可能的完成。An array variable from which Bash reads the possible completions generated by a shell function invoked by the programmable completion facility.</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">DIRSTACK</td>
			<td align="left" style="word-break: break-all; ">一个保存当前目录栈内容的变量数组。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">EUID</td>
			<td align="left" style="word-break: break-all; ">当前用户的数字有效用户ID。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">FCEDIT</td>
			<td align="left" style="word-break: break-all; ">内建命令&nbsp;<b>fc</b>&nbsp;使用 -e 选项时默认使用的编辑器。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">FIGNORE</td>
			<td align="left" style="word-break: break-all; ">一个由冒号分隔的在补全文件名时要忽略的后缀列表。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">FUNCNAME</td>
			<td align="left" style="word-break: break-all; ">任意当前正在执行的shell函数名。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">GLOBIGNORE</td>
			<td align="left" style="word-break: break-all; ">一个由冒号分隔的模板列表，定义在文件名展开时忽略的文件名集。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">GROUPS</td>
			<td align="left" style="word-break: break-all; ">一个数组变量，包含当前用户作为成员的组的列表。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">histchars</td>
			<td align="left" style="word-break: break-all; ">共有三个字符控制历史展开、快速替换和&nbsp;<i>标记</i>。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">HISTCMD</td>
			<td align="left" style="word-break: break-all; ">当前命令的历史数字，或者在历史列表里的索引。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">HISTCONTROL</td>
			<td align="left" style="word-break: break-all; ">定义一个命令是否加入到历史列表中。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">HISTFILE</td>
			<td align="left" style="word-break: break-all; ">保存历史命令的文件名。默认值是 ~/.bash_history。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">HISTFILESIZE</td>
			<td align="left" style="word-break: break-all; ">在历史文件中包含的最大行数，默认为500。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">HISTIGNORE</td>
			<td align="left" style="word-break: break-all; ">一个由冒号分隔的模式列表，用来决定哪些命令行应该保存在历史列表中。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">HISTSIZE</td>
			<td align="left" style="word-break: break-all; ">在历史列表中记录的最大命令数，默认为500。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">HOSTFILE</td>
			<td align="left" style="word-break: break-all; ">包含与 /etc/hosts 格式相同的文件名，shell需要补全主机名时读取。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">HOSTNAME</td>
			<td align="left" style="word-break: break-all; ">当前主机的名字。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">HOSTTYPE</td>
			<td align="left" style="word-break: break-all; ">一个描述运行Bash的机器的字符串。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">IGNOREEOF</td>
			<td align="left" style="word-break: break-all; ">控制shell接收到一个&nbsp;<i>EOF</i>&nbsp;字符作为独立输入的行为。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">INPUTRC</td>
			<td align="left" style="word-break: break-all; ">Readline初始化文件的名字，取代默认值为 /etc/inputrc。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">LANG</td>
			<td align="left" style="word-break: break-all; ">用于为任意没有特别选择用LC_开头的变量指明的设置决定场合设置。Used to determine the locale category for any category not specifically selected with a variable starting with LC_.</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">LC_ALL</td>
			<td align="left" style="word-break: break-all; ">这个变量取代 LANG 的值并为任意其他 LC_ 变量指定一个区域种类。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">LC_COLLATE</td>
			<td align="left" style="word-break: break-all; ">这个变量决定搜索文件名展开结果时使用的整理顺序，并决定在文件名展开和模式匹配里区域表达、等价类和处理序列的表现。This variable determines the collation order used when sorting the results of file name expansion, and determines the behavior of range expressions, equivalence classes, and collating sequences within file name expansion and pattern matching.</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">LC_CTYPE</td>
			<td align="left" style="word-break: break-all; ">这个变量决定在文件名展开和模板匹配里字符的解释和字符集的行为。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">LC_MESSAGES</td>
			<td align="left" style="word-break: break-all; ">这个变量决定用于转换由 &ldquo;$&rdquo; 引导的双引号字符串的区域。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">LC_NUMERIC</td>
			<td align="left" style="word-break: break-all; ">这个变量决定数字格式化的本地类别。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">LINENO</td>
			<td align="left" style="word-break: break-all; ">当前执行的脚本或者shell函数的行数。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">LINES</td>
			<td align="left" style="word-break: break-all; ">内建命令&nbsp;<b>select</b>&nbsp;用来决定打印选择列表的列长度。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">MACHTYPE</td>
			<td align="left" style="word-break: break-all; ">一个以标准的GNU CPU-COMPANY-SYSTEM 格式来充分描述运行Bash的系统的类型的字符串。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">MAILCHECK</td>
			<td align="left" style="word-break: break-all; ">shell从 MAILPATH 或 MAIL 变量里指定的文件中检查邮件的频度（秒）。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">OLDPWD</td>
			<td align="left" style="word-break: break-all; ">内建命令&nbsp;<b>cd</b>&nbsp;设置的之前的工作目录。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">OPTERR</td>
			<td align="left" style="word-break: break-all; ">如果设置成1，Bash显示内建命令&nbsp;<b>getopts</b>&nbsp;生成的错误信息。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">OSTYPE</td>
			<td align="left" style="word-break: break-all; ">一个描述运行Bash的操作系统的字符串。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">PIPESTATUS</td>
			<td align="left" style="word-break: break-all; ">一个数组变量，包含一个最近运行过的前台管道（可能只包含一个命令）进程的退出状态值的列表。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">POSIXLY_CORRECT</td>
			<td align="left" style="word-break: break-all; ">如果这个变量在&nbsp;<b>bash</b>&nbsp;启动的时候存在于环境变量中，shell将进入POSIX模式。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">PPID</td>
			<td align="left" style="word-break: break-all; ">shell父进程的进程ID。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">PROMPT_COMMAND</td>
			<td align="left" style="word-break: break-all; ">如果设置了，这个值解释为一个在打印每个基本提示符(PS1)之前执行的命令。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">PS3</td>
			<td align="left" style="word-break: break-all; ">这个变量的值被用作&nbsp;<b>select</b>&nbsp;命令的提示符。默认值是 &ldquo;&#39;#? &#39;&rdquo;。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">PS4</td>
			<td align="left" style="word-break: break-all; ">这个值在 -x 选项启用时，在命令行前打印的提示符。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">PWD</td>
			<td align="left" style="word-break: break-all; ">内建命令&nbsp;<b>cd</b>&nbsp;设置的当前工作目录。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">RANDOM</td>
			<td align="left" style="word-break: break-all; ">每次这个参数被引用时，生成一个0和32767之间的随机整数。给这个变量指定一个值作为随机数生成器的种子。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">REPLY</td>
			<td align="left" style="word-break: break-all; ">内建命令&nbsp;<b>read</b>&nbsp;的默认值。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">SECONDS</td>
			<td align="left" style="word-break: break-all; ">这个变量扩展为shell运行的秒数。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">SHELLOPTS</td>
			<td align="left" style="word-break: break-all; ">一个由冒号分隔的shell已经启用的选项列表。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">SHLVL</td>
			<td align="left" style="word-break: break-all; ">新的Bash实例启动就增加一。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">TIMEFORMAT</td>
			<td align="left" style="word-break: break-all; ">这个参数的值用来作为一个格式化的字符串用来指定以&nbsp;<b>time</b>&nbsp;保留字作为前缀的管道定时信息如何显示。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">TMOUT</td>
			<td align="left" style="word-break: break-all; ">如果设成一个大于0的值。 TMOUT 作为内建命令&nbsp;<b>read</b>&nbsp;的默认超时时间。在一个交互shell中，当shell处于交互状态时，这个值作为等待在基本提示串后输入的秒数。 在这个秒数后如果没有输入Bash就终止。</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; ">UID</td>
			<td align="left" style="word-break: break-all; ">数值，当前用户的真实用户ID。</td>
		</tr>
	</tbody>
</table>
<p>检查Bash的手册，信息或者文档页面得到更多扩展信息。一些变量是只读的，一些是自动设置的，另外一些如果设置了一个不同的值会失去原有的含义。</p>
