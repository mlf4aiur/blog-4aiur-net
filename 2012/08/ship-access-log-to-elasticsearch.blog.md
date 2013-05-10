Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2012/08/ship-access-log-to-elasticsearch/
Post: 908
Title: Ship access log to ElasticSearch
Slug: ship-access-log-to-elasticsearch
Postformat: standard
Keywords: ElasticSearch
Status: publish
Date: 2012-08-13 20:49:17 +0800
Pings: On
Comments: On
Category: Python

Ship access log to ElasticSearch
================================

This article introduce how to use a custom python script to parse Apache access log and shipping it to ElasticSearch.  
If you wan't store the huge log to ElasticSearch, you should read [Using Elasticsearch for logs](http://www.elasticsearch.org/tutorials/2012/05/19/elasticsearch-for-logging.html), Using some popular OpenSource software, like [Graylog2](http://graylog2.org/), [Logstash](http://logstash.net/), [Apache Flume](https://cwiki.apache.org/FLUME/).

System basic setup
------------------

<pre lang="bash">
# Raise the nofiles limit on Linux
if ! grep "\* *soft *nofile *65535" /etc/security/limits.conf; then
    cat >> /etc/security/limits.conf <<EOF
*               soft    nofile          65535
*               hard    nofile          65535
EOF
fi

# Setup java environment
echo "export JAVA_HOME=/usr/java/default" >> /etc/profile
echo "export PATH=$JAVA_HOME/bin:$PATH" >> /etc/profile
. /etc/profile
</pre>

Installation and Configuration ElasticSearch
--------------------------------------------

**Deploying ElasticSearch on a Cluster(EC2)**
<pre lang="bash">
cd /opt/
wget -O elasticsearch-0.19.8.tar.gz https://github.com/downloads/elasticsearch/elasticsearch/elasticsearch-0.19.8.tar.gz
tar zxf elasticsearch-0.19.8.tar.gz
mkdir -p /var/log/elasticsearch /var/data/elasticsearch
cd /opt/elasticsearch-0.19.8/
cat >> config/elasticsearch.yml <<EOF
cluster.name: yottaa_cluster
index.number_of_replicas: 2
path.data: /var/data/elasticsearch
path.logs: /var/log/elasticsearch
discovery.zen.ping.unicast.hosts: ["ip-10-196-37-204.ec2.internal[9300-9400]", "ip-10-116-113-224.ec2.internal[9300-9400]"]
EOF

# ElasticSearch, by default, binds itself to the 0.0.0.0 address, and listens
# on port [9200-9300] for HTTP traffic and on port [9300-9400] for node-to-node
# communication. (the range means that if the port is busy, it will automatically
# try the next port).

export ES_HOME=/opt/elasticsearch-0.19.8/
cd /opt/elasticsearch-0.19.8/bin
# modify memory size
sed 's/ES_MIN_MEM=[0-9].*/ES_MIN_MEM=1g/;s/ES_MAX_MEM=[0-9].*/ES_MAX_MEM=4g/' elasticsearch.in.sh > elasticsearch.sh
. /opt/elasticsearch-0.19.8/bin/elasticsearch.sh
# Install plugin elasticsearch-head
/opt/elasticsearch-0.19.8/bin/plugin -install mobz/elasticsearch-head
# Start ElasticSearch
/opt/elasticsearch-0.19.8/bin/elasticsearch
</pre>

**Cluster status**

<pre lang="bash">
curl -XGET 'http://*.*.*.*:9200/_cluster/health?pretty=true'; echo
curl -XGET 'http://*.*.*.*:9200/_cluster/state?pretty=true'; echo
curl -XGET 'http://*.*.*.*:9200/_cluster/nodes?pretty=true'; echo
curl -XGET 'http://*.*.*.*:9200/_cluster/nodes/stats?pretty=true'; echo
</pre>

Schema Mapping
--------------

<pre lang="bash">
curl -XPUT http://localhost:9200/_template/template_access/ -d '{
  "template": "access-*",
  "settings": { "number_of_replicas": 1, "number_of_shards": 5 },
  "mappings": {
    "access": {
      "_all": { "enabled": false },
      "_source": { "compress": true },
      "properties": {
        "bytes": { "index": "not_analyzed", "store": "yes", "type": "integer" },
        "host": { "index": "analyzed", "store": "yes", "type": "ip" },
        "method": { "index": "not_analyzed", "store": "yes", "type": "string" },
        "protocol": { "index": "not_analyzed", "store": "yes", "type": "string" },
        "referrer": { "index": "not_analyzed", "store": "yes", "type": "string" },
        "status": { "index": "analyzed", "store": "yes", "type": "string" },
        "timestamp": { "index": "analyzed", "store": "yes", "type": "date" },
        "uri": { "index": "not_analyzed", "store": "yes", "type": "string" },
        "user-agent": { "index": "not_analyzed", "store": "yes", "type": "string" }
      }
    }
  }
}'
</pre>

Ship log script
---------------

I have put this code on the github [source code](git://github.com/mlf4aiur/ship_log_to_elasticsearch.git).

**Usage:**

<pre lang="bash">cat /var/log/httpd/access_log | python ship_log_into_elasticsearch.py</pre>

or

<pre lang="bash">logtail /var/log/httpd/access_log | python ship_log_into_elasticsearch.py</pre>

config file:

<pre lang="bash">
cat conf/main.cfg
[elasticsearch]
host = localhost:9200
bulk_size = 5000
doc_type = access
</pre>

code:

<pre lang="python">
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import os
abspath = os.path.abspath(os.path.dirname(__file__))
os.chdir(abspath)
import sys
reload(sys)
sys.setdefaultencoding('utf-8')
import pyes
import ConfigParser
import time
import datetime
import re
import inspect
import logging
import logging.config
import traceback


# init logging facility
logconf = "conf/logging.cfg"
logging.config.fileConfig(logconf)


# 66.249.73.69 - - [08/Aug/2012:12:10:10 +0400] "GET / HTTP/1.1" 200 23920 "-" "Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)"
access_log_pattern = re.compile(
    r"(?P<host>[\d\.]+)\s"
    r"(?P<identity>\S*)\s"
    r"(?P<user>\S*)\s"
    r"(?P<time>\[.*?\])\s"
    r'"(?P<request>.*?)"\s'
    r"(?P<status>\d+)\s"
    r"(?P<bytes>\S*)\s"
    r'"(?P<referer>.*?)"\s'
    r'"(?P<user_agent>.*?)"\s*'
)


class HTTPDateTime(object):
    def __init__(self, time_string):
        """
        HTTP date time object
        @input time_string: [08/Aug/2012:12:10:10 +0400]
        """
        self.time_string = time_string
        self.altzone = time.altzone
        self.isodate = "%Y-%m-%dT%H:%M:%SZ"
        self.fmt = "[%d/%b/%Y:%H:%M:%S"
        return

    def to_unixtimestamp(self):
        (date_time, timezone) = self.time_string.split()
        offset = int(timezone[:-1]) / 100 * 60 * 60
        unixtimestamp = int(time.mktime(time.strptime(date_time, self.fmt)))
        unixtimestamp = unixtimestamp - self.altzone - offset
        return unixtimestamp

    def to_isodate(self):
        return time.strftime(self.isodate, time.gmtime(self.to_unixtimestamp()))


def log_entry_getter():
    for line in sys.stdin:
        match = access_log_pattern.match(line)
        if match:
            yield match.groupdict()


def log_doc_getter():
    logger = logging.getLogger(inspect.stack()[0][3])
    for fields in log_entry_getter():
        time = fields.get("time", "[01/Jan/1970:10:10:10 +0000]")
        http_datetime = HTTPDateTime(time)

        try:
            timestamp = http_datetime.to_isodate()
        except Exception:
            timestamp = "1970-01-01T10:10:10Z"

        try:
            request = fields.get("request")
            (method, uri, protocol) = request.split()
        except Exception:
            exstr = traceback.format_exc()
            logger.debug(request)
            logger.debug(exstr)
            continue

        doc = dict(host=fields.get("host", "1.1.1.1"),
                   timestamp=timestamp,
                   method=method,
                   uri=uri,
                   protocol=protocol,
                   status=fields.get("status", 000),
                   bytes=fields.get("bytes", 0),
                   referer=fields.get("referer", "-"),
                   user_agent=fields.get("user_agent", "-"))
        yield doc


def get_index_name():
    return datetime.datetime.now().strftime("access-%Y%m")


def create_index(conn, index_name):
    """
    Create an index
    dependent template:
    curl -XPUT http://localhost:9200/_template/template_access/ -d @conf/elasticsearch_template.json
    curl -XGET http://localhost:9200/_template/template_access/
    curl -XDELETE http://localhost:9200/_template/template_access/
    """
    try:
        conn.create_index(index_name)
    except pyes.exceptions.IndexAlreadyExistsException:
        pass


def put_index(conn, doc_type, mapping, index_name):
    conn.put_mapping(doc_type=doc_type,
                     mapping=mapping,
                     indices=[index_name])


def index_log(conn, index_name, doc_type):
    logger = logging.getLogger(inspect.stack()[0][3])
    for doc in log_doc_getter():
        try:
            conn.index(doc=doc, index=index_name, doc_type=doc_type, bulk=True)
        except Exception:
            exstr = traceback.format_exc()
            logger.warn(exstr)
            logger.warn("can not index: %s" % (str(doc)))
    return


def main():
    logger = logging.getLogger(inspect.stack()[0][3])
    logger.info("start")
    config = ConfigParser.RawConfigParser()
    config.read("conf/main.cfg")
    es_host = config.get("elasticsearch", "host").split(",")
    bulk_size = config.getint("elasticsearch", "bulk_size")
    doc_type = config.get("elasticsearch", "doc_type")

    # Creating a connection
    conn = pyes.ES(es_host, timeout=30.0, bulk_size=bulk_size)
    index_name = get_index_name()
    create_index(conn, index_name)
    put_index(conn, doc_type, None, index_name)
    index_log(conn, index_name, doc_type)
    conn.refresh([index_name])
    logger.info("done")
    return


if __name__ == "__main__":
    main()
</pre>

**Related links:**

* [elasticsearch](http://www.elasticsearch.org/), ElasticSearch is an Open Source (Apache 2), Distributed, RESTful, Search Engine built on top of Apache Lucene.
* [elasticsearch-head](http://mobz.github.com/elasticsearch-head/), elasticsearch-head is a web front end for browsing and interacting with an Elastic Search cluster.
* [pyes](https://github.com/aparo/pyes), pyes is a connector to use elasticsearch from python.
