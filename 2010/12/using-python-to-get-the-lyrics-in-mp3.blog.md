Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/12/using-python-to-get-the-lyrics-in-mp3/
Post: 299
Title: 使用python获取mp3中的歌词
Slug: using-python-to-get-the-lyrics-in-mp3
Postformat: standard
Keywords: Python
Status: publish
Date: 2010-12-09 12:50:25 +0800
Pings: On
Comments: On
Category: Python

**使用python获取mp3中的歌词**

安装依赖库eyeD3

<pre lang="bash">$ sudo port install py26-eyed3</pre>

<pre lang="python">#!/usr/bin/env python
# -*- coding: utf-8 -*-
"""
dump_mp3_lyris.py

Created by 4Aiur on 2010-12-09.
"""

import os
import eyeD3

def get_mp3_file(dir):
    mp3_files = []
    files = os.listdir(dir)
    for file in files:
        if os.path.splitext(file)[1] == '.mp3':
            mp3_files.append(os.path.join(dir,file))
    return mp3_files

def dump_lyrics(mp3_file):
    print('parse %s' % (mp3_file))
    lyrics_file = '%s.txt' % (os.path.splitext(mp3_file)[0])
    fp = open(lyrics_file, 'w')
    tag = eyeD3.Tag()
    tag.link(mp3_file)
    fp.write('Title: %s\n\n' % (tag.getTitle().encode('utf-8', 'ignore')))
    lyrics = tag.getLyrics()
    for item in lyrics:
        for line in item.lyrics.splitlines():
            result = line.encode('utf-8', 'ignore')
            fp.write( result + '\n')
    fp.close()
    print('write %s done.' % (lyrics_file))
    return

def main():
    mp3_files = get_mp3_file('/Users/4aiur/Shares/englishpod')
    for mp3_file in mp3_files:
        dump_lyrics(mp3_file)

if __name__ == '__main__':
    main()
</pre>
