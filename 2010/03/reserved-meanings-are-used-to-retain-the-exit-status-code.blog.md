Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/03/reserved-meanings-are-used-to-retain-the-exit-status-code/
Post: 55
Title: 被用于保留(reserved meanings)的退出状态码
Slug: reserved-meanings-are-used-to-retain-the-exit-status-code
Postformat: standard
Keywords: shell
Status: publish
Date: 2010-03-31 21:11:35 +0800
Pings: On
Comments: On
Category: Shell

<p>&nbsp;</p>
<h4>Exit Codes With Special Meanings</h4>
<h4>Reserved Exit Codes</h4>
<hr style="height: 1px; border-top-width: 1px; border-right-width: 0px; border-bottom-width: 0px; border-left-width: 0px; border-style: initial; border-color: initial; border-top-style: solid; border-top-color: rgb(204, 204, 204); " />
<table border="1" style="font-family: Tahoma, Arial; color: rgb(0, 0, 0); font-size: 12px; ">
	<tbody>
		<tr>
			<th align="left" valign="top">Exit Code Number</th>
			<th align="left" valign="top">Meaning</th>
			<th align="left" valign="top">Example</th>
			<th align="left" valign="top">Comments</th>
		</tr>
	</tbody>
	<tbody>
		<tr>
			<td align="left" style="word-break: break-all; " valign="top">1</td>
			<td align="left" style="word-break: break-all; " valign="top">Catchall for general errors</td>
			<td align="left" style="word-break: break-all; " valign="top">let &quot;var1 = 1/0&quot;</td>
			<td align="left" style="word-break: break-all; " valign="top">Miscellaneous errors, such as &quot;divide by zero&quot;</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; " valign="top">2</td>
			<td align="left" style="word-break: break-all; " valign="top">Misuse of shell builtins (according to Bash documentation)</td>
			<td align="left" style="word-break: break-all; " valign="top">?</td>
			<td align="left" style="word-break: break-all; " valign="top">Seldom seen, usually defaults to exit code 1</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; " valign="top">126</td>
			<td align="left" style="word-break: break-all; " valign="top">Command invoked cannot execute</td>
			<td align="left" style="word-break: break-all; " valign="top">?</td>
			<td align="left" style="word-break: break-all; " valign="top">Permission problem or command is not an executable</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; " valign="top">127</td>
			<td align="left" style="word-break: break-all; " valign="top">&quot;command not found&quot;</td>
			<td align="left" style="word-break: break-all; " valign="top">?</td>
			<td align="left" style="word-break: break-all; " valign="top">Possible problem with $PATH or a typo</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; " valign="top">128</td>
			<td align="left" style="word-break: break-all; " valign="top">Invalid argument to exit</td>
			<td align="left" style="word-break: break-all; " valign="top">exit 3.14159</td>
			<td align="left" style="word-break: break-all; " valign="top"><strong>exit</strong>&nbsp;takes only integer args in the range 0 - 255 (see footnote)</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; " valign="top">128+n</td>
			<td align="left" style="word-break: break-all; " valign="top">Fatal error signal &quot;n&quot;</td>
			<td align="left" style="word-break: break-all; " valign="top"><strong>kill -9</strong>&nbsp;$PPID of script</td>
			<td align="left" style="word-break: break-all; " valign="top"><strong>$?</strong>&nbsp;returns 137 (128 + 9)</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; " valign="top">130</td>
			<td align="left" style="word-break: break-all; " valign="top">Script terminated by Control-C</td>
			<td align="left" style="word-break: break-all; " valign="top">?</td>
			<td align="left" style="word-break: break-all; " valign="top">Control-C is fatal error signal 2, (130 = 128 + 2, see above)</td>
		</tr>
		<tr>
			<td align="left" style="word-break: break-all; " valign="top">255*</td>
			<td align="left" style="word-break: break-all; " valign="top">Exit status out of range</td>
			<td align="left" style="word-break: break-all; " valign="top">exit -1</td>
			<td align="left" style="word-break: break-all; " valign="top"><strong>exit</strong>&nbsp;takes only integer args in the range 0 - 255</td>
		</tr>
	</tbody>
</table>
<hr style="height: 1px; border-top-width: 1px; border-right-width: 0px; border-bottom-width: 0px; border-left-width: 0px; border-style: initial; border-color: initial; border-top-style: solid; border-top-color: rgb(204, 204, 204); " />
<div>According to the above table, exit codes 1 - 2, 126 - 165, and 255 have special meanings, and should therefore be avoided for user-specified exit parameters. Ending a script with&nbsp;<strong>exit 127</strong>would certainly cause confusion when troubleshooting (is the error code a &quot;command not found&quot; or a user-defined one?). However, many scripts use an&nbsp;<strong>exit 1</strong>&nbsp;as a general bailout upon error. Since exit code 1 signifies so many possible errors, this probably would not be helpful in debugging.</div>
<div>There has been an attempt to systematize exit status numbers (see /usr/include/sysexits.h), but this is intended for C and C++ programmers. A similar standard for scripting might be appropriate. The author of this document proposes restricting user-defined exit codes to the range 64 - 113 (in addition to 0, for success), to conform with the C/C++ standard. This would allot 50 valid codes, and make troubleshooting scripts more straightforward.</div>
<div>All user-defined exit codes in the accompanying examples to this document now conform to this standard, except where overriding circumstances exist, as in Example 9-2.</div>
<div>Issuing a $? from the command line after a shell script exits gives results consistent with the table above only from the Bash or&nbsp;<em>sh</em>&nbsp;prompt. Running the&nbsp;<em>C-shell</em>&nbsp;or&nbsp;<em>tcsh</em>&nbsp;may give different values in some cases.</div>
<h3>Notes</h3>
<div>Out of range exit values can result in unexpected exit codes. An exit value greater than 255 returns an exit code modulo 256. For example,&nbsp;<strong>exit 3809</strong>&nbsp;gives an exit code of 225 (3809 % 256 = 225).</div>
