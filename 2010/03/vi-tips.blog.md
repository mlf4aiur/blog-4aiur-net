Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/03/vi-tips/
Post: 21
Title: vi使用小贴士
Slug: vi-tips
Postformat: standard
Keywords: vi
Status: publish
Date: 2010-03-31 16:22:39 +0800
Pings: On
Comments: On
Category: Linux

**vi使用小贴士**

<pre>进入vi的命令
vi filename :打开或新建文件，并将光标置于第一行首
vi +n filename ：打开文件，并将光标置于第n行首
vi + filename ：打开文件，并将光标置于最后一行首
vi +/pattern filename：打开文件，并将光标置于第一个与pattern匹配的串处
vi -r filename ：在上次正用vi编辑时发生系统崩溃，恢复filename
vi filename....filename ：打开多个文件，依次编辑

h ：光标左移一个字符
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
$：光标移至当前行尾

屏幕翻滚类命令
Ctrl+u：向文件首翻半屏
Ctrl+d：向文件尾翻半屏
Ctrl+f：向文件尾翻一屏
Ctrl＋b；向文件首翻一屏
nz：将第n行滚至屏幕顶部，不指定n时将当前行滚至屏幕顶部。

插入文本类命令
i ：在光标前
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
nCC：修改指定数目的行

删除命令
ndw或ndW：删除光标处开始及其后的n-1个字
d0：删至行首
d$：删至行尾
ndd：删除当前行及其后n-1行
x或X：删除一个字符，x删除光标后的，而X删除光标前的
Ctrl+u：删除输入方式下所输入的文本

搜索及替换命令 :
/pattern：从光标开始处向文件尾搜索pattern
?pattern：从光标开始处向文件首搜索pattern
n：在同一方向重复上一次搜索命令
N：在反方向上重复上一次搜索命令
：s/p1/p2/g：将当前行中所有p1均用p2替代
：n1,n2s/p1/p2/g：将第n1至n2行中所有p1均用p2替代
：g/p1/s//p2/g：将文件中所有p1均用p2替换</pre>

VI tips

1. 交换两个字符位置
xp
2. 上下两行调换
ddp
3. 上下两行合并
J
4. 删除所有行
dG
5. 从当前位置删除到行尾
d$
6. 从当前位置复制到行尾
y$ 如果要粘贴到其他地方 p 就可以了
7. :ab string strings
例如 “:ab jn Ji Nan” ,
当你在文见里插入 jn 时 ，Ji Nan 就蹦出来了
8. 单个字符替换用r，覆盖多个字符用R，用多个字符替换一个字符用s，整行替换用S
