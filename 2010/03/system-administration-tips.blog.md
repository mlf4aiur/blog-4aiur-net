Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/03/system-administration-tips/
Post: 10
Title: 系统管理的几个小技巧
Slug: system-administration-tips
Postformat: standard
Keywords: Linux, shell
Status: publish
Date: 2010-03-31 15:59:10 +0800
Pings: On
Comments: On
Category: Linux

**系统管理的几个小技巧**

一、SecureCRT的使用技巧

1、由于网络条件问题，SecureCRT经常会在不使用的时候开服务器的连接。

用下面的方法设置会很大程度上解决上面的问题。（以SecureCRT Version 5.1.0为例）

打开SecureCRT选择Options的General,点Default Session之后选择Edit Default Settions  
再选择Terminal,把Send protocol NO_OP 选上之后全部点ok即可。  

注：  
同样利用screen命令也可以  
使用方法  
SSH登录后执行  
`# screen -S freebsd`  
一旦断线可使用  
`# screen -x freebsd`  
来恢复

2、密钥的使用

使用密钥便于连接与管理服务器，也极大的增加了系统的安全性。  
个人建议禁止root直接登陆服务器，禁用密码直接登陆服务器，安装密钥时设置密码。  
具体使用方法可以到网上搜集资料，这里不再多说。  

3、与服务器之间的文件传输

设置连接的属性  
选中一连接后鼠标右键点Properties，选择第一项Connection，把Protocol里面的Terminal设置为SSH2，File设置为SFTP（注需要事先安装SecureFX，否则没有次选项）  
进入一个连接后点File菜单，点Connect SFTP Tab（快捷键为alt+p）,会打开SFTP窗口，可以使用命令传输文件。  
命令举例：  

* lcd 进入本地目录
* lpwd 显示本地目录路径
* cd 进入服务器本身的目录
* pwd 显示服务器本身的路径
* get 下载服务器文件至本地目录
* put 上传本地文件至服务器

SecureFX是一个使用sftp协议的客户端程序，使用方法与其他ftp客户端类似。

4、使用鼠标键的左键复制与中键粘贴

二、关于BASH命令行使用的

输入set -o emacs为使用emacs（默认），输入set -o vi为使用vi为编辑器。  
我们经常用的是emacs的方式控制命令行，下面是常用快捷键的解释。

<pre>Ctrl-P      Move up history file.
Ctrl-N      Move down history file.
Ctrl-B      Move backward one character.
Ctrl-R      Search backward for string.
Esc B      Move backward one word.
Ctrl-F      Move forward one character.
Esc F      Move forward one word.
Ctrl-A      Move to the beginning of the line.
Ctrl-E      Move to the end of the line.
Esc <      Move to the first line of the history file.
Esc >      Move to the last line of the history file.
Editing with emacs:
Ctrl-U      Delete the line.
Ctrl-Y      Put the line back.
Ctrl-K      Delete from cursor to the end line.
Ctrl-D      Delete a letter.
Esc D      Delete one word forward.
Esc H      Delete one word backward.
Esc space      Set a mark at cursor position.
Ctrl-X Ctrl-X      Exchange cursor and mark.
Ctrl-P Ctrl-Y      Push region from cursor to mark into a buffer (Ctrl-P) and put it down (Ctrl-Y).</pre>

三、vi的使用

进入vi的命令

<pre>vi filename :打开或新建文件，并将光标置于第一行首
vi +n filename ：打开文件，并将光标置于第n行首
vi + filename ：打开文件，并将光标置于最后一行首
vi +/pattern filename：打开文件，并将光标置于第一个与pattern匹配的串处
vi -r filename ：在上次正用vi编辑时发生系统崩溃，恢复filename
vi filename....filename ：打开多个文件，依次编辑</pre>

<pre>h ：光标左移一个字符
l ：光标右移一个字符
space：光标右移一个字符
Backspace：光标左移一个字符
k或Ctrl+p：光标上移一行
j或Ctrl+n ：光标下移一行
Enter ：光标下移一行
w或W ：光标右移一个字至字首
b或B ：光标左移一个字至字首
e或E ：光标右移一个字j至字尾
) ：光标移至句尾
( ：光标移至句首
}：光标移至段落开头
{：光标移至段落结尾
nG：光标移至第n行首
n+：光标下移n行
n-：光标上移n行
n$：光标移至第n行尾
H ：光标移至屏幕顶行
M ：光标移至屏幕中间行
L ：光标移至屏幕最后行
0：（注意是数字零）光标移至当前行首
$：光标移至当前行尾</pre>

屏幕翻滚类命令

<pre>Ctrl+u：向文件首翻半屏
Ctrl+d：向文件尾翻半屏
Ctrl+f：向文件尾翻一屏
Ctrl＋b；向文件首翻一屏
nz：将第n行滚至屏幕顶部，不指定n时将当前行滚至屏幕顶部。</pre>

插入文本类命令

<pre>i ：在光标前
I ：在当前行首
a：光标后
A：在当前行尾
o：在当前行之下新开一行
O：在当前行之上新开一行
r：替换当前字符
R：替换当前字符及其后的字符，直至按ESC键
s：从当前光标位置处开始，以输入的文本替代指定数目的字符
S：删除指定数目的行，并以所输入文本代替之
ncw或nCW：修改指定数目的字
nCC：修改指定数目的行</pre>

删除命令

<pre>ndw或ndW：删除光标处开始及其后的n-1个字
d0：删至行首
d$：删至行尾
ndd：删除当前行及其后n-1行
x或X：删除一个字符，x删除光标后的，而X删除光标前的
Ctrl+u：删除输入方式下所输入的文本</pre>

搜索及替换命令 :

<pre>/pattern：从光标开始处向文件尾搜索pattern
?pattern：从光标开始处向文件首搜索pattern
n：在同一方向重复上一次搜索命令
N：在反方向上重复上一次搜索命令
：s/p1/p2/g：将当前行中所有p1均用p2替代
：n1,n2s/p1/p2/g：将第n1至n2行中所有p1均用p2替代
：g/p1/s//p2/g：将文件中所有p1均用p2替换</pre>

VI tips

1. 交换两个字符位置 xp
2. 上下两行调换 ddp
3. 上下两行合并 J
4. 删除所有行 dG
5. 从当前位置删除到行尾 d$
6. 从当前位置复制到行尾 y$ 如果要粘贴到其他地方 p 就可以了
7. :ab string strings  
例如 “:ab jn Ji Nan” ,  
当你在文见里插入 jn 时 ，Ji Nan 就蹦出来了
8. 单个字符替换用r，覆盖多个字符用R，用多个字符替换一个字符用s，整行替换用S
