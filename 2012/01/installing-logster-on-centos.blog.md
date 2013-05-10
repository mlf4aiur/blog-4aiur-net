Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2012/01/installing-logster-on-centos/
Post: 656
Title: Installing Logster on CentOS
Slug: installing-logster-on-centos
Postformat: standard
Keywords: logster
Status: publish
Date: 2012-01-10 22:57:52 +0800
Pings: On
Comments: On
Category: SysAdmin

# Installing Logster on CentOS

### Install EPEL repository
<pre lang="bash">
sudo rpm -Uvh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-6.noarch.rpm
yum update # This takes quite a while for a fresh install
</pre>

### Setting locale
<pre lang="bash">
cat >> ~/.bash_profile << EOF
export LANGUAGE=en_US.UTF-8
export LANG=en_US.UTF-8
export LC_ALL=en_US.UTF-8
EOF
source .bash_profile
</pre>

### Install Dependence
<pre lang="bash">yum -y install logcheck</pre>

logcheck dependencies:

* liblockfile
* lockfile-progs
* perl-IPC-Signal
* perl-Proc-WaitStat
* perl-mime-construct

### Install logster
<pre lang="bash">
git clone git://github.com/etsy/logster.git
cd logster
make install
</pre>

**dry run**

<pre lang="bash">/usr/sbin/logster --output=stdout SampleLogster /var/log/httpd/access_log</pre>

### Add crontab
<pre lang="bash">
crontab -e
* * * * * /usr/sbin/logster -p blog_4aiur_net --output=graphite --graphite-host=localhost:2003 SampleLogster /var/log/httpd/blog.4aiur.net_access_log >/dev/null 2>&1
</pre>
