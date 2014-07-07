Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2012/01/installing-and-configuring-graphite-on-centos/
Post: 652
Title: Installing and Configuring Graphite on CentOS
Slug: installing-and-configuring-graphite-on-centos
Postformat: standard
Keywords: graphite
Status: publish
Date: 2012-01-10 22:51:24 +0800
Pings: On
Comments: On
Category: SysAdmin

Installing and Configuring Graphite on CentOS
=============================================

Install EPEL repository
-----------------------

<pre lang="bash">
sudo rpm -Uvh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
sudo yum -y update # This takes quite a while for a fresh install
</pre>

Alternatively:

<pre lang="bash">
wget -r -l1 --no-parent -A "epel*.rpm" http://dl.fedoraproject.org/pub/epel/6/x86_64/
sudo yum -y --nogpgcheck localinstall */pub/epel/6/x86_64/epel-*.rpm
</pre>

Install Dependences
-------------------

<pre lang="bash">
sudo yum -y install gcc.x86_64 git.x86_64 python-devel pyOpenSSL \
    python-memcached bitmap bitmap-fonts python-crypto zope pycairo \
    mod_python python-ldap Django django-tagging python-sqlite2
</pre>

Install Python package management software
------------------------------------------

<pre lang="bash">
sudo yum -y install python-pip.noarch
</pre>

Install Graphite
----------------

* carbon

    a Twisted daemon that listens for time-series data

* whisper

    a simple database library for storing time-series data (similar in design to RRD)

* graphite-web

    a Django webapp that renders graphs on-demand using Cairo

<pre lang="bash">
sudo pip-python install whisper carbon graphite-web 
</pre>

Configure graphite
------------------

<pre lang="bash">
cd /opt/graphite/conf
sudo rename .conf.example .conf *
cd /opt/graphite/webapp/graphite
sudo python manage.py syncdb
sudo chown -R apache:apache /opt/graphite/storage/
</pre>

Start the data collection daemon carbon-cache
---------------------------------------------

<pre lang="bash">
cd /opt/graphite/bin
sudo ./carbon-cache.py start
</pre>

Configure Apache VirtualHost
----------------------------

edit /etc/httpd/conf/httpd.conf

<pre lang="xml">
<VirtualHost *:80>
    ServerName graphite.4aiur.net
    DocumentRoot "/opt/graphite/webapp"
    CustomLog /var/log/httpd/graphite.4aiur.net_access_log combined

    <Location "/">
        SetHandler python-program
        PythonPath "['/opt/graphite/webapp'] + ['/usr/lib/python/site-packages/'] + sys.path"
        PythonHandler django.core.handlers.modpython
        SetEnv DJANGO_SETTINGS_MODULE graphite.settings
        PythonDebug Off
        PythonAutoReload Off
    </Location>
    
    <Location "/content/">
        SetHandler None
    </Location>
    
    <Location "/media/">
        SetHandler None
    </Location>
    alias /media/ /usr/lib/python2.6/site-packages/django/contrib/admin/media/
</VirtualHost>
</pre>

Test insert data to graphite
----------------------------

**run test**
<pre lang="bash">
python /opt/graphite/examples/example-client.py
</pre>

**example-client.py source code**
<pre lang="python">
#!/usr/bin/python
"""Copyright 2008 Orbitz WorldWide

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License."""

import sys
import time
import os
import platform
import subprocess
from socket import socket

CARBON_SERVER = '127.0.0.1'
CARBON_PORT = 2003

delay = 60
if len(sys.argv) > 1:
  delay = int( sys.argv[1] )

def get_loadavg():
  # For more details, "man proc" and "man uptime"
  if platform.system() == "Linux":
    return open('/proc/loadavg').read().strip().split()[:3]
  else:
    command = "uptime"
    process = subprocess.Popen(command, stdout=subprocess.PIPE, shell=True)
    os.waitpid(process.pid, 0)
    output = process.stdout.read().replace(',', ' ').strip().split()
    length = len(output)
    return output[length - 3:length]

sock = socket()
try:
  sock.connect( (CARBON_SERVER,CARBON_PORT) )
except:
  print "Couldn't connect to %(server)s on port %(port)d, is carbon-agent.py running?" % { 'server':CARBON_SERVER, 'port':CARBON_PORT }
  sys.exit(1)

while True:
  now = int( time.time() )
  lines = []
  #We're gonna report all three loadavg values
  loadavg = get_loadavg()
  lines.append("system.loadavg_1min %s %d" % (loadavg[0],now))
  lines.append("system.loadavg_5min %s %d" % (loadavg[1],now))
  lines.append("system.loadavg_15min %s %d" % (loadavg[2],now))
  message = '\n'.join(lines) + '\n' #all lines must end in a newline
  print "sending message\n"
  print '-' * 80
  print message
  print
  sock.sendall(message)
  time.sleep(delay)
</pre>

View graphite data
------------------

<pre lang="bash">
sudo iptables -I INPUT -p tcp --dport 80 -j ACCEPT
sudo service httpd restart
</pre>

goto your graphite site

Delete graphite data
--------------------

<pre lang="bash">
cd /opt/graphite/storage/whisper
rm your_data.wsp
</pre>

Some wonderful Graphite dashboards
----------------------------------

* <https://github.com/torkelo/grafana>
* <https://github.com/urbanairship/tessera>
* <https://github.com/ripienaar/gdash>
* <https://github.com/Dieterbe/graph-explorer>
* <https://github.com/obfuscurity/descartes>
* <https://github.com/obfuscurity/tasseo>
* <http://kenhub.github.com/giraffe>
* <http://jondot.github.com/graphene/>
* <http://square.github.com/cubism/>
