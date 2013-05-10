Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/03/digital-print-format/
Post: 78
Title: 数字的格式化打印
Slug: digital-print-format
Postformat: standard
Keywords: shell
Status: publish
Date: 2010-03-31 21:41:06 +0800
Pings: On
Comments: On
Category: SysAdmin

**数字的格式化打印**

有人问我一个产生"01 02 03 04 05 06 07 08 09 10 11 12 13 14 15 16 17 18 19 20"这样一种字符串的方法，当时我随意的写了一个

<pre lang="bash">for ((x=1;x<=20;x++))
do
    if [ ${#x} -eq 1 ]; then
        echo -n "0$x "
    else
        echo -n "$x "
    fi
done

01 02 03 04 05 06 07 08 09 10 11 12 13 14 15 16 17 18 19 20</pre>

我当时觉得有点土，之后人家问了如果打印的结果是从0到100怎么办呢？

因为最近买了本python的书，在复习pyhon的基础知识，恰好在看格式胡print的时候看到print%0是用0替换空格来占位，当时看书的时候感到有点怪，不知道数字前面加0有什么用，所以还有点印象。

所以像下面的python语句那么写好看多了。

<pre lang="python">Python 2.6.2 (r262:71600, Jul 2 2009, 16:30:16)

[GCC 3.4.4 20050721 (Red Hat 3.4.4-2)] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> for x in range(20): print "%02d" %x,
...
00 01 02 03 04 05 06 07 08 09 10 11 12 13 14 15 16 17 18 19
>>></pre>

之前printf用的很少，但是也清楚shell的格式化打印也是C的printf like的，今天到公司后试了下printf，确实可以work。

<pre lang="bash">for ((x=1;x<=20;x++)); do printf "%02d " $x; done
01 02 03 04 05 06 07 08 09 10 11 12 13 14 15 16 17 18 19 20</pre>

bash 4的新功能

<pre lang="bash">[4aiur@FreeBSD ~]$ bash --version
GNU bash, version 4.0.33(0)-release (i386-portbld-freebsd8.0)
Copyright (C) 2009 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gp...
This is free software; you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
[4aiur@FreeBSD ~]$ echo {01..10}
01 02 03 04 05 06 07 08 09 10</pre>

C

<pre lang="bash">#include <stdio.h>
int main(void){
    int x;
    for (x = 0; x <=20 ; x++) {
        printf("%02d\n", x);
    }
    return 0;
}</pre>

shell

<pre lang="bash">seq -w 1 20
seq -f '%02g' 1 20</pre>
