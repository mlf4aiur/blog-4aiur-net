Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2012/01/jmx-monitoring/
Post: 647
Title: JMX monitoring
Slug: jmx-monitoring
Postformat: standard
Status: publish
Date: 2012-01-09 12:14:09 +0800
Pings: On
Comments: On
Category: Default

# JMX monitoring

### dataflow

<pre lang="bash">
java app -> jmxagent ->
                         jmxtrans -> jmx_output.log -> alert.sh -> send mail
java app -> jmxagent ->
</pre>

### Configuration jmx agent with authentication

JMX dynamically allocated random port, and it will bind the port at internal address. 
If you connecting jmx through firewall or your servers on Amazon EC2, 
maybe can't connect to the jmx agent, So need to do some prepare.

**add jmx agent parameter in your java startup script**  

<pre lang="bash">cmd[0]='curl -s http://ifconfig.me/ip 2>/dev/null | tr -d "\n"'
cmd[1]='curl -s http://sputnick-area.net/ip 2>/dev/null'
for ((x=0; x<${#cmd[@]}; x++))
do
    tmp=$(echo ${cmd[$x]} | sh)
    if echo $tmp | grep -q "^\([0-9]\{1,3\}\.\)\{3\}[0-9]\{1,3\}$"; then
        external_ip=$tmp
        break
    fi
done
-Drmi.agent.port=1234 -javaagent:/opt/app/plugins/jmx-agent.jar \
-Djmx.remote.x.password.file=/opt/app/conf/jmxremote.password \
-Djmx.remote.x.access.file=/opt/app/conf/jmxremote.access \
-Djava.rmi.server.hostname=${external_ip:-127.0.0.1}</pre>

configuration jmx authentication

<pre lang="bash">
cat > /opt/app/conf/jmxremote.access << EOF
monitor   readonly
EOF
cat > /opt/app/conf/jmxremote.password << EOF
monitor  yourmonitoringaccesspassword
EOF
chmod 600 /opt/app/conf/jmxremote.access /opt/app/conf/jmxremote.password</pre>

build JMXAgent.class to jmx-agent.jar

<pre lang="java">package example.rmi.agent;

import java.io.IOException;
import java.lang.management.ManagementFactory;
import java.net.InetAddress;
import java.net.UnknownHostException;
import java.rmi.registry.LocateRegistry;
import java.util.HashMap;

import javax.management.MBeanServer;
import javax.management.remote.JMXConnectorServer;
import javax.management.remote.JMXConnectorServerFactory;
import javax.management.remote.JMXServiceURL;

/**
 * This class is used for the resolve the
 * "Connecting Through Firewall Using JMX" issue.
 * 
 * http://blogs.oracle.com/jmxetc/entry/connecting_through_firewall_using_jmx
 * 
 * @author root
 * 
 */
public class JMXAgent {
	
	
	private static int _rmiRegistryPort=3000;
	
	

	public static void premain(String agentArgs) throws IOException {

		// Ensure cryptographically strong random number generator used
		// to choose the object number - see java.rmi.server.ObjID
		//
		System.setProperty("java.rmi.server.randomIDs", "true");

		// Start an RMI registry on port specified by example.rmi.agent.port
		// (default 3000).
		//
		final int port = Integer.parseInt(System.getProperty(
				"yottaa.rmi.agent.port", String.valueOf(_rmiRegistryPort)));

		System.out.println("Create RMI registry on port " + port);
		LocateRegistry.createRegistry(port);

		// Retrieve the PlatformMBeanServer.
		//
		System.out.println("Get the platform's MBean server");
		MBeanServer mbs = ManagementFactory.getPlatformMBeanServer();

		// Environment map.
		//
		System.out.println("Initialize the environment map");
		HashMap<String, Object> env = new HashMap<String, Object>();

		// This where we would enable security - left out of this
		// for the sake of the example....
		//

		// Create an RMI connector server.
		//
		// As specified in the JMXServiceURL the RMIServer stub will be
		// registered in the RMI registry running in the local host on
		// port 3000 with the name "jmxrmi". This is the same name the
		// out-of-the-box management agent uses to register the RMIServer
		// stub too.
		//
		// The port specified in "service:jmx:rmi://"+hostname+":"+port
		// is the second port, where RMI connection objects will be exported.
		// Here we use the same port as that we choose for the RMI registry.
		// The port for the RMI registry is specified in the second part
		// of the URL, in "rmi://"+hostname+":"+port
		//
		System.out.println("Create an RMI connector server");
		final String hostname = InetAddress.getLocalHost().getHostName();
		JMXServiceURL url = new JMXServiceURL("service:jmx:rmi://" + hostname
				+ ":" + port + "/jndi/rmi://" + hostname + ":" + port
				+ "/jmxrmi");

		// Now create the server from the JMXServiceURL
		//
		JMXConnectorServer cs = JMXConnectorServerFactory.newJMXConnectorServer(url, env, mbs);

		// Start the RMI connector server.
		//
		System.out.println("Start the RMI connector server on port " + port);
		cs.start();
	}
	
}</pre>

### Install jmxtrans on your alert server

<pre lang="bash">rpm -Uvh http://jmxtrans.googlecode.com/files/jmxtrans-250-0.noarch.rpm
ln -s /usr/java/jdk1.6.0_27/bin/jps /usr/bin/jps</pre>

### Configuration jmxtrans

<pre lang="bash">cat > /var/lib/jmxtrans/monitoring.json << \EOF
{
    "servers": [
        {
            "host": "hostA", 
            "port": "1234", 
            "username": "monitor", 
            "password": "yourmonitoringaccesspassword", 
            "numQueryThreads": 2, 
            "queries": [
                {
                    "attr": [
                        "HeapMemoryUsage", 
                        "NonHeapMemoryUsage"
                    ], 
                    "obj": "java.lang:type=Memory", 
                    "output.jsontWriters": [
                        {
                            "@class": "com.googlecode.jmxtrans.model.output.jsont.KeyOutWriter", 
                            "settings": {
                                "debug": true, 
                                "maxLogBackupFiles": 3, 
                                "maxLogFileSize": "10MB", 
                                "output.jsontFile": "/var/log/jmxtrans/jmx_output.log", 
                                "typeNames": ["name"]
                            }
                        }
                    ]
                }
            ]
        }, 
        {
            "host": "hostB", 
            "port": "1234", 
            "username": "monitor", 
            "password": "yourmonitoringaccesspassword", 
            "numQueryThreads": 2, 
            "queries": [
                {
                    "attr": [
                        "HeapMemoryUsage", 
                        "NonHeapMemoryUsage"
                    ], 
                    "obj": "java.lang:type=Memory", 
                    "output.jsontWriters": [
                        {
                            "@class": "com.googlecode.jmxtrans.model.output.jsont.KeyOutWriter", 
                            "settings": {
                                "debug": true, 
                                "maxLogBackupFiles": 3, 
                                "maxLogFileSize": "10MB", 
                                "output.jsontFile": "/var/log/jmxtrans/jmx_output.log", 
                                "typeNames": ["name"]
                            }
                        }
                    ]
                }
            ]
        }
    ]
}
EOF</pre>

### Running Jmx Transformer

<pre lang="bash">service jmxtrans start
# or run jmxtrans manually
cd /usr/share/jmxtrans/
/usr/share/jmxtrans/jmxtrans.sh start /var/lib/jmxtrans/monitoring.json
/usr/share/jmxtrans/jmxtrans.sh stop /var/lib/jmxtrans/monitoring.json</pre>

### View jmxtrans log

<pre lang="bash">cd /var/log/jmxtrans
tailf jmxtrans.log</pre>

### Alert script thresholds config

<pre lang="bash">cat > thresholds.conf << EOF
#parttern expression value
\.\.HeapMemoryUsage_used Maximum 2500000000
\.\.HeapMemoryUsage_committed Minimum 6000000000
EOF</pre>

### Alert script

<pre lang="bash">cat > /opt/alert.sh << \EOF
#!/usr/bin/env bash

# configs
source_file="/var/log/jmxtrans/jmx_output.log"
max_line=500
now=$(date +%s)
expire=61
to_addr="your@email.address"
TAC="/usr/bin/tac"

# functions
logger () {
    datetime=$(date +"%F %T")
    echo ${datetime}: $*
}

send_mail () {
    message=$*
    logger send email
    logger $message
    #echo $message | mail -s "jmx alert" ${to_addr}
}

# main 
logger "start."
message=$($TAC $source_file 2>/dev/null | head -${max_line} | \
    awk -v Now=$now -v Expire=$expire '
BEGIN{
    # configuration
    # #parttern expression value
    # HeapMemoryUsage_used Maximum 2500000000
    # HeapMemoryUsage_committed Minimum 6000000000
    while (getline < "thresholds.conf") {
        if ($0 ~ "#") {
            continue
        } else {
            Partterns[$1] = $1
            Expressions[$1] = $2
            Values[$1] = $3
        }
    }
}
# input: hostA_1234.sun_management_MemoryImpl..HeapMemoryUsage_used 734907264   1325840738049
function scanner (Attr, Value) {
    for (Parttern in Partterns) {
        if (Attr ~ Parttern) {
            if (Expressions[Parttern] == "Maximum") {
                if (Value >= Values[Parttern]) {
                    return Hit=1
                }
            } else if (Expressions[Parttern] == "Minimum") {
                if (Value <= Values[Parttern]) {
                    return Hit=1
                }
            }
        }
    }
    return Hit=0
}
{
    if (NF!=3) {
        next
    } else {
        Attr = $1
        Value = $2
        TimeStamp = int($3/1000)
        if ((TimeStamp+Expire) >= Now) {
            scanner(Attr, Value)
            if (Hit == 1) {
                print
                exit
            }
        } else {
            exit
        }
    }
}
')

if [[ ! -z $message ]]; then
    send_mail "$message"
fi
logger "done."
EOF
chmod +x /opt/alert.sh</pre>

### Add script into crontab

<pre lang="bash">crontab -l > tmp.cron
echo "* * * * * /opt/alert.sh >> /var/log/jmxtrans/alert.log 2>&1" >> tmp.cron
crontab tmp.cron
rm -f tmp.cron</pre>

### References

* <http://blogs.oracle.com/jmxetc/entry/connecting_through_firewall_using_jmx>
* <http://code.google.com/p/jmxtrans/wiki/KeyOutWriter>
