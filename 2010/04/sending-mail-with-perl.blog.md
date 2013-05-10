Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/04/sending-mail-with-perl/
Post: 117
Title: perl发送邮件注意事项
Slug: sending-mail-with-perl
Postformat: standard
Keywords: Perl
Status: publish
Date: 2010-04-06 11:16:20 +0800
Pings: On
Comments: On
Category: Perl

**perl发送邮件注意事项**

1) 程序手工执行可以发送mail，而把程序加入到crontab后无法执行的解决方法在crontab中执行脚本前，加入`. /etc/profile`;

<pre lang="bash">0 * * * * * . /etc/profile ; /usr/bin/perl /app_path/mail.pl >/dev/null 2>&1</pre>

2) smtp服务器需要认证，而Net::SMTP的auth方法不起作用的解决方法Net::SMTP的auth方法依赖Authen::SASL这个模块，
可以用`perl -e 'use Authen::SASL'`试试，看下是否报错，如果报错的话需要安装Authen::SASL模块。

安装方法：

<pre lang="bash"># perl -MCPAN -e 'install Authen::SASL'</pre>

顺便提一下，用抓包工具看的话smtp认证过程账户和密码部分是乱码，其实可以转换出来，乱码的unicode是base64,使用python的decode方法可以转换出明文。

<pre lang="bash">>>> s='string'
>>> s.encode('base64')
'c3RyaW5n\n'
>>> u=u'c3RyaW5n\n'
>>> u.decode('base64')
'string'
>>> </pre>

unicode的问题在RFC 2554中有所描述。  
附代码：

<pre lang="perl">#!/usr/bin/perl

use strict;
use warnings;
use Net::SMTP;

my $mailhost='mail.example.com'; #Mail Server .
my $mailfrom='foo@example.com'; #Mail from
my @mailto=('bar@example.com'); #Receive list
my $subject='subject: Topic';

my $text='content';

my $smtp=Net::SMTP->new($mailhost,Timeout=>120,Debug=>1) or die "Error.\n";
$smtp->auth('foo','passwd'); #username and password are needed

foreach my $mailto(@mailto) {
$smtp->mail($mailfrom);
$smtp->to($mailto);

$smtp->data();

$smtp->datasend("To: $mailto\n");
$smtp->datasend("From:$mailfrom\n");
$smtp->datasend("Subject: $subject\n");
$smtp->datasend("\n");

#Message
$smtp->datasend("$text\n\n");

$smtp->dataend();
}
$smtp->quit;</pre>
