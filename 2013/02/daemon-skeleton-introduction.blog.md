Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2013/02/daemon-skeleton-introduction/
Post: 924
Title: Daemon Skeleton introduction
Slug: daemon-skeleton-introduction
Postformat: standard
Keywords: Daemon, Python
Status: publish
Date: 2013-02-24 22:20:44 +0800
Pings: On
Comments: On
Category: Python

Daemon Skeleton introduction
============================

Past couple of days, i've builded some daemon tools, tired to do the same thing again and again, i think i need a template, to create python project with daemon easy and fast. So created this template [Daemon Skeleton](https://github.com/mlf4aiur/daemon-skeleton).

In this template i used [python-daemon](https://github.com/serverdensity/python-daemon) as the daemonizer, and did some tweak, add SIGQUIT signal catching in this module, and reformat the source code according to [PEP8](http://www.python.org/dev/peps/pep-0008/).

Daemon Skeleton Quick start
---------------------------

Clone the git repository, remove folder .git, create isolated Python environment with virturalenv, install dependencies:

<pre lang="bash">
git clone https://github.com/mlf4aiur/daemon-skeleton.git
rm -rf .git
sudo pip install virtuall
virtualenv .venv
. .venv/bin/activate
pip install -r requirements.txt
</pre>

Then, done. You can continue your daemon project now.

Another things for build good Python project
--------------------------------------------

**Automation**

* Create your own requirements.txt, when you need

<pre lang="bash">
pip freeze | awk -F = '{print $1}' > requirements.txt
</pre>

* Upgrade the packages to latest version

<pre lang="bash">
pip install -U -r requirements.txt
</pre>

* create a new source distribution as tarball

<pre lang="bash">
python setup.py sdist --formats=gztar
# Fabric
fab pack
</pre>

* Deploying your code automatically

<pre lang="bash">
fab pack deploy
</pre>

* Testing code

<pre lang="bash">
# nose
nosetests
# Fabric
fab test
# setuptools
python setup.py test
</pre>

**Good Documentation**

<pre lang="bash">
pip install Sphinx
sphinx-quickstart
# Build docs
python setup.py build_sphinx
</pre>

Related links 
-------------
* [distutils](http://docs.python.org/distutils/sourcedist.html)
* [virtualenv](https://pypi.python.org/pypi/virtualenv)
* [Fabric](http://fabfile.org/)
* [Sphinx](http://sphinx-doc.org)
* [Restructured Text (reST) and Sphinx CheatSheet](http://openalea.gforge.inria.fr/doc/openalea/doc/_build/html/source/sphinx/rest_syntax.html)
