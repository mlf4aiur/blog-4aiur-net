Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/12/opensolaris-rsync-install-script/
Post: 310
Title: OpenSolaris rsync install script
Slug: opensolaris-rsync-install-script
Postformat: standard
Keywords: OpenSolaris, rsync
Status: publish
Date: 2010-12-21 17:08:04 +0800
Pings: On
Comments: On
Category: OpenSolaris

**OpenSolaris rsync install script**

<pre lang="bash">#!/bin/bash
# OpenSolaris rsync installer
# Created by 4Aiur on 2010-12-21.

# define function
check_rsync_config () {
    if [ ! -d /opt/local/etc/rsync/ ]; then
        mkdir -p /opt/local/etc/rsync/
    fi
    
    if [ ! -f /opt/local/etc/rsync/rsyncd.conf ]; then
        cat > /opt/local/etc/rsync/rsyncd.conf <<\EOF
#maximum allowed connections
max connections = 10
#where to log
log file = /var/log/rsync.log
timeout = 300

[update]
comment = update downloads
path = /home/foo/rsync_data/
read only = false
list = yes
uid = foo
gid = foo 
hosts allow = 192.168.1.0/24
secrets file = /opt/local/etc/rsync/rsyncd.secrets
auth users = foo #enter username specified in secrets file
EOF
    fi
    if [ ! -f /opt/local/etc/rsync/rsyncd.secrets ]; then
        cat > /opt/local/etc/rsync/rsyncd.secrets <<\EOF
rsync:rsync_pass
EOF
        chmod 600 /opt/local/etc/rsync/rsyncd.secrets
    fi
}

check_svc_rsync () {
    if [ ! -f /var/svc/manifest/network/rsync.xml ]; then
        cat > /var/svc/manifest/network/rsync.xml <<\EOF
<?xml version="1.0"?>
<!DOCTYPE service_bundle SYSTEM "/usr/share/lib/xml/dtd/service_bundle.dtd.1">

<service_bundle type='manifest' name='rsync'>

  <service name='network/rsync' type='service' version='4'>

    <create_default_instance enabled='false'/>

    <single_instance/>

    <!--
    If there's no network, then there's no point in running 
    -->
    <dependency
      name='loopback'
      grouping='require_all'
      restart_on='error'
      type='service'>
      <service_fmri value='svc:/network/loopback:default'/>
    </dependency>

    <dependency
      name='physical'
      grouping='require_all'
      restart_on='error'
      type='service'>
      <service_fmri value='svc:/network/physical:default'/>
    </dependency>
    <dependency
      name='config_data'
      grouping='require_all'
      restart_on='restart'
      type='path'>
      <service_fmri value='file://localhost//opt/local/etc/rsync/rsyncd.conf'/>
    </dependency>
    <dependency
      name='fs-local'
      grouping='require_all'
      restart_on='none'
      type='service'>
      <service_fmri value='svc:/system/filesystem/local'/>
    </dependency>

    <exec_method
      type='method'
      name='start'
      exec='/opt/local/bin/rsync --daemon'
      timeout_seconds='60'/>

    <exec_method
      type='method'
      name='stop'
      exec=':kill'
      timeout_seconds='60'/>

    <exec_method
      type='method'
      name='refresh'
      exec=':kill -HUP'
      timeout_seconds='60'/>

    <stability value='Unstable'/>

    <template>
      <common_name>
        <loctext xml:lang='C'>RSYNC daemon</loctext>
      </common_name>

      <documentation>
        <manpage title='rsync' section='7'/>
        <doc_link name='rsync.org' uri='http://www.rsync.org/docs/'/>
      </documentation>
    </template>
  </service>
</service_bundle>
EOF
    fi
}

rotate_log () {
    logadm -w rsync -s 10m -C 10 -t '/var/log/rsync.log.%Y-%m' \
    -a '/usr/sbin/svcadm restart rsync' /var/log/rsync.log
}

# Main
echo "setup rsync server start"
# Check rsync package
if ! pkgin list | grep rsync >/dev/null; then
    pkgin -y in rsync
fi

# Check rsync server config file
check_rsync_config

# Check rsync service config
check_svc_rsync

# Import rsync.xml
if ! svcs -a | grep net-snmp >/dev/null; then
    svccfg import /var/svc/manifest/network/rsync.xml
fi
svcadm enable rsync

# Add log rotate config
rotate_log

echo "setup rsync server done"</pre>
