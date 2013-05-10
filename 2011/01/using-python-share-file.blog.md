Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2011/01/using-python-share-file/
Post: 330
Title: 使用python实现简易的文件共享
Slug: using-python-share-file
Postformat: standard
Keywords: CGI, Python
Status: publish
Date: 2011-01-21 16:29:27 +0800
Pings: On
Comments: Off
Category: Python

**使用python实现简易的文件共享**

可以利用python的SimpleHTTPServer共享自己的文件给其他人

<pre lang="bash">python -m SimpleHTTPServer 8000</pre>

需要从其他人那拷贝东西到自己机器上时可以用python的CGIHTTPServer这个模块，写一个CGI程序来接收文件。

<pre lang="bash"># alias
alias cgiserver="ifconfig | grep --color -o 'inet 1[79]2[^ ]*'; python -m CGIHTTPServer 8000" # cgi_directories ['/cgi-bin', '/htbin']

# Create directory
mkdir incoming/
cd incoming/
mkdir cgi-bin/ files</pre>

<pre lang="python"># Create save file CGI program
cat > cgi-bin/save_file.py <<\EOF
#!/usr/bin/env python
import cgi, os
import cgitb; cgitb.enable()

try: # Windows needs stdio set for binary mode.
    import msvcrt
    msvcrt.setmode (0, os.O_BINARY) # stdin  = 0
    msvcrt.setmode (1, os.O_BINARY) # stdout = 1
except ImportError:
    pass

form = cgi.FieldStorage()

# Generator to buffer file chunks
def fbuffer(f, chunk_size=10000):
   while True:
      chunk = f.read(chunk_size)
      if not chunk: break
      yield chunk

# A nested FieldStorage instance holds the file
fileitem = form['file']

# Test if the file was uploaded
if fileitem.filename:
   
   # strip leading path from file name to avoid directory traversal attacks
   fn = os.path.basename(fileitem.filename)
   f = open('files/' + fn, 'wb')

   # Read the file in chunks
   for chunk in fbuffer(fileitem.file):
      f.write(chunk)
   f.close()
   message = 'The file "' + fn + '" was uploaded successfully'
   
else:
   message = 'No file was uploaded'
   
print """\
Content-Type: text/html\n
<html><body>
<p>%s</p>
</body></html>
""" % (message,)
EOF

chmod +x cgi-bin/save_file.py

# Create upload.html file
cat > upload.html <<\EOF
<html><body>
<form enctype="multipart/form-data" action="cgi-bin/save_file.py" method="post">
<p>File: <input type="file" name="file"></p>
<p><input type="submit" value="Upload"></p>
</form>
</body></html>
EOF
</pre>

<pre lang="bash">cgiserver
</pre>

其他人通过访问http://uripaddress:8000/upload.html来上传文件。
