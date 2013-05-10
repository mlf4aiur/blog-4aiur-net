Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/12/using-python-to-download-the-resources-stored-in-the-index/
Post: 289
Title: 使用python下载存放在Index中的资源
Slug: using-python-to-download-the-resources-stored-in-the-index
Postformat: standard
Keywords: Python
Status: publish
Date: 2010-12-06 16:47:20 +0800
Pings: On
Comments: On
Category: Python

**使用python下载存放在Index中的资源**

Useing python threading download example.
like this:

<pre lang="bash">wget -P result/ -nd -nH -r -l 1 -np -A pdf --http-user=book --http-password=cubook "http://www.4aiur.net/book/Programing/Python/"</pre>

<pre lang="python">#!/usr/bin/env python
# -*- coding: utf-8 -*-

'''
thread_download.py
下载网页中某种后缀名的文件
Created by 4Aiur on 2010-12-06.
'''
__version__ = "$Revision: 0.90 $"

import threading
import httplib
import urlparse
import base64
import re
import os
from BeautifulSoup import BeautifulSoup
from Queue import Queue

download_queue = Queue()

def get_object_url(start_url, username, password, suffix):
    ''' 抓取并解析index文件，分析页面获取需要下载指定
    后缀名的元素 '''
    s = urlparse.urlparse(start_url)
    host = s.netloc
    path = s.path
    url_object = set()
    conn = httplib.HTTPConnection(host)
    params = None
    base64string = base64.encodestring('%s:%s' % (username, password))[:-1]
    headers = {'User-agent': '4Aiur Crawler',
            'Authorization': 'Basic %s' % base64string}
    conn.request('GET', path, params, headers)
    response = conn.getresponse()
    content = response.read()
    content = unicode(content, errors="ignore")
    conn.close()
    soup = BeautifulSoup()
    soup.feed(content)
    items = soup.findAll('a')
    for item in items:
        href = item.get('href')
        if href.endswith(suffix):
            full_url = urlparse.urljoin(start_url, href)
            url_object.add(full_url)
    return url_object 

def download(n, save_path):
    '''download object'''
    while True:
        download_task = download_queue.get()
        host = download_task.host
        path = download_task.path
        username = download_task.username
        password = download_task.password
        re_separate = re.compile(r'/(?:/)*')
        file_name = re.split(re_separate, path)[-1]
        local_file = os.path.join(save_path, file_name)
        print('Thread-%d: downlaod %s to %s' % (n, download_task.url, local_file))
        params = None
        base64string = base64.encodestring('%s:%s' % (username, password))[:-1]
        headers = {'User-agent': '4Aiur Crawler',
                'Authorization': 'Basic %s' % base64string}
        write_buffer = 104857600
        # 避免线程内代码出现异常，导致线程无法退出，把执行代码放入到异常判断中
        try:
            conn = httplib.HTTPConnection(host)
            conn.request('GET', path, params, headers)
            response = conn.getresponse()
            fp = open(local_file, 'wb')
            while True:
                block = response.read(write_buffer)
                if block:
                    fp.write(block)
                else:
                    break
            fp.close()
        finally:
            download_queue.task_done()

class DownloadObject(object):
    def __init__(self, url, username, password):
        self.url = url
        s = urlparse.urlparse(url)
        self.host = s.netloc
        self.path = s.path
        self.username = username
        self.password = password
        return

def main():
    num_threads = 3
    save_path = 'result'
    if not os.path.isdir(save_path):
        os.makedirs(save_path)
    # 开启下载线程
    for n in range(num_threads):
        t = threading.Thread(target=download, args=(n, save_path))
        t.setDaemon(True)
        t.start()

    start_url = 'http://www.4aiur.net/book/Programing/Python/'
    username = 'book'
    password = 'cubook'
    suffix = 'pdf'
    for url in get_object_url(start_url, username, password, suffix):
        download_queue.put(DownloadObject(url, username, password))
    # 等待线程结束
    download_queue.join()
    return

if __name__ == '__main__':
    main()
</pre>
