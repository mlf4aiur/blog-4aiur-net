Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/06/install-and-configuration-proftpd-on-opensolaris/
Post: 196
Title: 在OpenSolaris上安装与配置proftpd
Slug: install-and-configuration-proftpd-on-opensolaris
Postformat: standard
Keywords: OpenSolaris, proftpd
Status: publish
Date: 2010-06-17 15:29:31 +0800
Pings: Off
Comments: On
Category: OpenSolaris

**在OpenSolaris上安装与配置proftpd**

### Install proftpd

<pre lang="bash"># pkgin in proftpd</pre>

**Setup proftpd**  
**proftpd configuration examples /opt/local/share/examples/proftpd**  
**modify /opt/local/etc/proftpd.conf**

<pre lang="html">TimeoutLogin         120
TimeoutIdle          600
TimeoutNoTransfer    900
TimeoutStalled       3600
RootLogin            off
DefaultRoot ~
UseReverseDNS        off
IdentLookups         off

#VALID LOGINS
<limit LOGIN>
  AllowUser ftpuser
  DenyALL
</limit></pre>

### Setup openSolaris SMF
<pre lang="bash"># vi /var/svc/manifest/network/proftpd.xml</pre>
<pre lang="html"><?xml version="1.0"?>
<!DOCTYPE service_bundle SYSTEM "/usr/share/lib/xml/dtd/service_bundle.dtd.1">
<service_bundle type='manifest' name='export'>
<service name='network/proftpd' type='service' version='0'>
<instance name='proftpd' enabled='false'>
<dependency name='network' grouping='require_all' restart_on='error' type='service'>
<service_fmri value='svc:/milestone/network:default'/>
</dependency>
<dependency name='filesystem-local' grouping='require_all' restart_on='none' type='service'>
<service_fmri value='svc:/system/filesystem/local:default'/>
</dependency>
<exec_method name='start' type='method' exec='/usr/local/sbin/in.proftpd' timeout_seconds='10'>
<method_context/>
</exec_method>
<exec_method name='stop' type='method' exec='/usr/bin/pkill -9 -U root proftpd' timeout_seconds='5'>
<method_context/>
</exec_method>
<exec_method name='refresh' type='method' exec='/usr/bin/pkill -1 -U root proftpd' timeout_seconds='5'>
<method_context/>
</exec_method>
<property_group name='proftpd' type='application'>
<stability value='Evolving'/>
<propval name='ssl' type='boolean' value='false'/>
</property_group>
<property_group name='startd' type='framework'>
<propval name='ignore_error' type='astring' value='core,signal'/>
</property_group>
</instance>
<stability value='Evolving'/>
<template>
<common_name>
<loctext xml:lang='C'>ProFTPD Server</loctext>
</common_name>
<documentation>
<manpage title='proftpd' section='8' manpath='/usr/local/man'/>
<doc_link name='www.proftpd.org' uri='http://www.proftpd.org'/>
</documentation>
</template>
</service>
</service_bundle></pre>

<pre lang="bash"># svccfg import /var/svc/manifest/network/proftpd.xml
# svcadm enable proftpd
# svcs proftpd</pre>
