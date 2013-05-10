Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/12/opensolaris-net-snmp-install-script/
Post: 303
Title: OpenSolaris net-snmp install script
Slug: opensolaris-net-snmp-install-script
Postformat: standard
Keywords: net-snmp, OpenSolaris
Status: publish
Date: 2010-12-16 14:31:45 +0800
Pings: On
Comments: On
Category: OpenSolaris

**OpenSolaris net-snmp install script**

<pre lang="bash">#!/bin/bash
# OpenSolaris net-snmp installer
# Created by 4Aiur on 2010-12-14.

# define function
check_netsnmp_config () {
    if [ ! -f /var/net-snmp/snmpd.local.conf ]; then
        cat > /var/net-snmp/snmpd.local.conf <<\EOF
###############################################################################
# Access Control
###############################################################################
####
# First, map the community name (COMMUNITY) into a security name
# (local and mynetwork, depending on where the request is coming
# from):
#       sec.name  source          community
com2sec local     localhost       public
com2sec mynetwork NETWORK/24      COMMUNITY

####
# Second, map the security names into group names:
#               sec.model  sec.name
group MyRWGroup v2c        local
group MyRWGroup usm        local
group MyROGroup v2c        mynetwork
group MyROGroup usm        mynetwork

####
# Third, create a view for us to let the groups have rights to:
#           incl/excl subtree                          mask
view all    included  .1.3.6.1.4.1.2021
view all    included  .1.3.6.1.2.1.1
view all    included  .1.3.6.1.2.1.2.2
view all    included  .1.3.6.1.2.1.25.1.1               

####
# Finally, grant the 2 groups access to the 1 view with different
# write permissions:
#                context sec.model sec.level match  read   write  notif
access MyROGroup ""      any       noauth    exact  all    none   none
access MyRWGroup ""      any       noauth    exact  all    all    none

###############################################################################
# System contact information
syslocation Right here, right now.
syscontact Me <me@somewhere.org>

###############################################################################
# Process checks.
# Make sure sshd is running
proc sshd

###############################################################################
# disk checks
# disk PATH [MIN=DEFDISKMINIMUMSPACE]
# Check the / partition and make sure it contains at least 20 percent.
disk / 20%

###############################################################################
# load average checks
# load [1MAX=DEFMAXLOADAVE] [5MAX=DEFMAXLOADAVE] [15MAX=DEFMAXLOADAVE]
load 12 14 14

###############################################################################
# Executables/scripts
# exec NAME PROGRAM [ARGS ...]
exec echotest /bin/echo hello world
EOF
    fi
    chmod 600 /var/net-snmp/snmpd.local.conf
}

check_svc_netsnmp () {
    if [ ! -f /var/svc/manifest/application/management/net-snmp.xml ]; then
        cat > /var/svc/manifest/application/management/net-snmp.xml <<\EOF
<?xml version="1.0"?>
<!DOCTYPE service_bundle SYSTEM "/usr/share/lib/xml/dtd/service_bundle.dtd.1">
<!--
    CDDL HEADER START
   
    The contents of this file are subject to the terms of the
    Common Development and Distribution License (the "License").
    You may not use this file except in compliance with the License.
   
    You can obtain a copy of the license at usr/src/OPENSOLARIS.LICENSE
    or http://www.opensolaris.org/os/licensing.
    See the License for the specific language governing permissions
    and limitations under the License.
   
    When distributing Covered Code, include this CDDL HEADER in each
    file and include the License file at usr/src/OPENSOLARIS.LICENSE.
    If applicable, add the following below this CDDL HEADER, with the
    fields enclosed by brackets "[]" replaced with your own identifying
    information: Portions Copyright [yyyy] [name of copyright owner]
   
    CDDL HEADER END
   
    Copyright 2009 Sun Microsystems, Inc.  All rights reserved.
    Use is subject to license terms.

    ident    "@(#)net-snmp.xml    1.1    09/07/06 SMI"

    NOTE:  This service description is not editable; its contents
    may be overwritten by package or patch operations, including
    operating system upgrade.  Make customizations in a different
    file.

    Service manifest for the net-snmp daemon
-->

<service_bundle type='manifest' name='SUNWnet-snmp-core:net-snmp'>

<service
    name='application/management/net-snmp'
    type='service'
    version='1'>

    <create_default_instance enabled='false' />

    <single_instance />

    <dependency
        name='milestone'
        grouping='require_all'
        restart_on='none'
        type='service'> 
        <service_fmri value='svc:/milestone/sysconfig' />
    </dependency>

    <!-- Need / & /usr filesystems mounted, /var mounted read/write -->
    <dependency
        name='fs-local'
        type='service'
        grouping='require_all'
        restart_on='none'>
            <service_fmri value='svc:/system/filesystem/local' />
    </dependency>

    <dependency
        name='name-services'
        grouping='optional_all'
        restart_on='none'
        type='service'>
        <service_fmri value='svc:/milestone/name-services' />
    </dependency>

    <dependency
        name='system-log'
        grouping='optional_all'
        restart_on='none'
        type='service'>
        <service_fmri value='svc:/system/system-log' />
    </dependency>

    <dependency
        name='rstat'
        grouping='optional_all'
        restart_on='none'
        type='service'>
        <service_fmri value='svc:/network/rpc/rstat' />
    </dependency>

    <dependency name='cryptosvc'
        grouping='require_all'
        restart_on='restart'
        type='service'>
            <service_fmri value='svc:/system/cryptosvc' />
    </dependency>

    <dependency
        name='network'
        grouping='require_all'
        restart_on='restart'
        type='service'>
            <service_fmri value='svc:/milestone/network' />
    </dependency>

    <dependency
        name='config-file'
        grouping='require_all'
        restart_on='refresh'
        type='path'>
            <service_fmri 
               value='file://localhost/var/net-snmp/snmpd.local.conf' />
    </dependency>

    <exec_method
            type='method'
        name='start'
        exec='/lib/svc/method/svc-net-snmp'
        timeout_seconds='60'>
    </exec_method>

    <exec_method
       type='method'
       name='stop'
       exec=':kill'
       timeout_seconds='60'>
    </exec_method>

    <exec_method
       type='method'
       name='refresh'
       exec=':kill -HUP'
       timeout_seconds='60'>
    </exec_method>

    <property_group name='general' type='framework'>
        <!-- to start/stop net-snmp -->
        <propval name='action_authorization' type='astring'
            value='solaris.smf.manage.net-snmp' />
        <propval name='value_authorization' type='astring'
            value='solaris.smf.manage.net-snmp' />
        <propval name='arch_type' type='integer' value='0' />
    </property_group>
    
    <stability value='Unstable' />

    <template>
        <common_name>
            <loctext xml:lang='C'>
            net-snmp SNMP daemon
            </loctext>
        </common_name>

        <documentation>
            <manpage title='snmpd' section='8' 
                manpath='/usr/share/man/' />
        </documentation>

    </template>

</service>

</service_bundle>
EOF
    fi
}
# Check net-snmp package
if ! pkgin list | grep net-snmp >/dev/null; then
    pkgin -y in net-snmp
fi

# Check directory
if [ ! -d /var/net-snmp/ ]; then
    mkdir -p /var/net-snmp/
fi

# Check net-snmp config file.
check_netsnmp_config

# Check net-snmp service config
check_svc_netsnmp

# Import net-snmp.xml
if ! svcs -a | grep net-snmp >/dev/null; then
    svccfg import /var/svc/manifest/application/management/net-snmp.xml
fi
svcadm enable net-snmp
sleep 3
echo "Install net-snmp done."
echo "check net-snmp log."
tail /var/svc/log/application-management-net-snmp:default.log
echo "lookup net-snmp listen port"
netstat -an -P udp | grep 161
</pre>
