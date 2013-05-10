Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/12/logrotate-on-opensolaris/
Post: 307
Title: Logrotate on OpenSolaris
Slug: logrotate-on-opensolaris
Postformat: standard
Keywords: logrotate, OpenSolaris
Status: publish
Date: 2010-12-21 17:03:20 +0800
Pings: On
Comments: On
Category: OpenSolaris

**Logrotate on OpenSolaris**

On OpenSolaris, you use the logadm command, with the actual rotation being specified in /etc/logadm.conf

<pre lang="bash">logadm -v -w rsync -s 10m -C 10 \
-t '/var/log/rsync.log.%Y-%m' \
-a '/usr/sbin/svcadm restart rsync' \
/var/log/rsync.log
</pre>
Testing with logadm -p now rsync seems to work just fine.
