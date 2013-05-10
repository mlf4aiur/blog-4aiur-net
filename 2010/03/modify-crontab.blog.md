Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/03/modify-crontab/
Post: 58
Title: 自动修改crontab配置
Slug: modify-crontab
Postformat: standard
Keywords: Linux, shell
Status: publish
Date: 2010-03-31 21:13:46 +0800
Pings: On
Comments: On
Category: Shell

**自动修改crontab配置**

方法1:使用crontab -l把crontab内容导出到文件中，使用编辑器或脚本修改导出的文件，之后使用新的配置文件覆盖掉现有的配置。

<pre lang="bash">[4Aur@4Aiur ~]$ crontab -l > cron[4Aur@4Aiur ~]$ sed -i 's/ls/dir/' cron
[4Aur@4Aiur ~]$ crontab cron</pre>

方法2:使用here文档的方式更新crontab的配置。

<pre lang="bash">[4Aur@4Aiur ~]$ crontab -l0 * * * * ls
[4Aur@4Aiur ~]$ sh cron_modify.sh
[4Aur@4Aiur ~]$ crontab -l
0 * * * * dir

[4Aur@4Aiur ~]$ cat cron_modify.sh
crontab -e <<\EOF &>/dev/null
:%s/ls/dir/g
:wq
EOF
[4Aur@4Aiur ~]$</pre>
