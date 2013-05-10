Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2012/04/deploy-webpy-on-openshift/
Post: 700
Title: Deploy web.py application on OpenShift
Slug: deploy-webpy-on-openshift
Postformat: standard
Keywords: OpenShift, web.py
Status: publish
Date: 2012-04-03 16:18:10 +0800
Pings: On
Comments: On
Category: Python

Deploy web.py application on OpenShift
=======

### Prepare environment

**Install OpenShift client command line tool**

<pre lang="bash">gem install rhc</pre>

**Generate a new SSH key**

<pre lang="bash">ssh-keygen -t rsa -f ~/.ssh/rhcloud -C "your_email@youremail.com"</pre>

**edit ~/.ssh/config**

<pre lang="bash">Host *.rhcloud.com
    IdentityFile ~/.ssh/rhcloud</pre>

**Create a Namespace**

<pre lang="bash">rhc-create-domain -n namespace -a ~/.ssh/rhcloud -l rhlogin</pre>

**Create a application, and clone the git reposity**
<pre lang="bash">rhc-create-app -a webpy -t python-2.6 -l rhlogin</pre>

### Write your application

**edit setup.py**

<pre lang="bash">install_requires=['web.py>=0.36'],</pre>

**edit wsgi/application**
<pre lang="python">#!/usr/bin/python
import os

virtenv = os.environ['APPDIR'] + '/virtenv/'
os.environ['PYTHON_EGG_CACHE'] = os.path.join(virtenv, 'lib/python2.6/site-packages')
virtualenv = os.path.join(virtenv, 'bin/activate_this.py')
try:
    execfile(virtualenv, dict(__file__=virtualenv))
except IOError:
    pass

import web

urls = (
        '/', 'index',
        '/hello/(.*)', 'hello'
)

class index:
    def GET(self):
        return 'Welcome to my web site!'

class hello:
    def GET(self, name):
        if not name:
            name = 'World'
        return 'Hello, ' + name + '!'

application = web.application(urls, globals()).wsgifunc()

#
# Below for testing only
#
if __name__ == '__main__':
    from wsgiref.simple_server import make_server
    httpd = make_server('localhost', 8080, application)
    # Wait for a single request, serve it and quit.
    httpd.handle_request()
    # app = web.application(urls, globals())
    # app.run()</pre>

**Commit and push your code**
<pre lang="bash">git status
git add setup.py wsgi/application
git commit -m "first commit"
git push</pre>

**Bind your Domain**
<pre lang="bash">rhc-ctl-app -a webpy -l rhlogin -c add-alias --alias yourdomain</pre>

Using browser access your web site, Enjoy!
