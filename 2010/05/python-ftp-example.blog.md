Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/05/python-ftp-example/
Post: 162
Title: python的ftp应用举例
Slug: python-ftp-example
Postformat: standard
Keywords: Python
Status: publish
Date: 2010-05-04 14:20:42 +0800
Pings: Off
Comments: On
Category: Python

**python的ftp应用举例**

使用python可以很方便的进行ftp操作，下面这个小程序的作用是成功下载远程服务器文件后，删除远程服务器上面的文件。

<pre lang="python">#!/usr/bin/env python2
# -*- coding: utf-8 -*-

from ftplib import FTP
import logging
import os
import socket

logger = logging.getLogger("download")
logger.setLevel(logging.DEBUG)
fh = logging.FileHandler("download.log")
formatter = logging.Formatter("%(asctime)s - %(name)s - %(levelname)s - %(message)s")
fh.setFormatter(formatter)
logger.addHandler(fh)

def get_del():
    ip = '127.0.0.1'
    user_name = 'foo'
    passwd = 'bar'
    source_path = '/test_sour_dir/'
    try:
        ftp = FTP(ip)
        timeout=10 # in seconds
        socket.setdefaulttimeout(timeout)
        ftp.set_pasv(True)
        ftp.login(user_name, passwd)
        logger.info('login %s success' %(ip))
    except Exception, e:
        logger.error('Can\'t connect ftp server, %s' %(e))
        return
    try:
        ftp.cwd(source_path)
        remote_files = ftp.nlst()
    except Exception, e:
        logger.error('ftp error, %s' %(e))
        return
    try:
        for file_name in remote_files:
            remote_file_size = ftp.size(file_name)
            f = open('/test_dest_dir/' + file_name, 'wb')
            logger.debug('%s download success' %(file_name))
            ftp.retrbinary('RETR %s' %(file_name), f.write, 4096)
            f.close()
            local_file_size = os.stat('/test_dest_dir/' + file_name).st_size
            logger.debug('%s remote size: %d, local size %d' %(file_name, remote_file_size, local_file_size))
            ftp.delete(file_name)
            logger.debug('%s delete success' %(file_name))
    except Exception, e:
        logger.error('ftp error, %s' %(e))
        return
    ftp.close()
    return

if __name__ == "__main__":
    get_del()
</pre>
