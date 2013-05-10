Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/04/using-python-to-send-mail-with-attachments/
Post: 135
Title: 使用python发送带附件的邮件
Slug: using-python-to-send-mail-with-attachments
Postformat: standard
Keywords: email, Python
Status: publish
Date: 2010-04-06 11:27:37 +0800
Pings: On
Comments: On
Category: Python

**使用python发送带附件的邮件**

<pre lang="python">#!/usr/bin/env python
# -*- coding: utf-8 -*-

from email.MIMEText import MIMEText
from email.MIMEMultipart import MIMEMultipart
from email.MIMEBase import MIMEBase
from email import Utils, Encoders
import mimetypes, sys
import smtplib
import re
import datetime


def attachment(filename):
   fd = open(filename, "rb")
   mimetype, mimeencoding = mimetypes.guess_type(filename)
   if mimeencoding or (mimetype is None):
       mimetype = "application/octet-stream"
   maintype, subtype = mimetype.split("/")
   if maintype == "text":
       retval = MIMEText(fd.read(), _subtype=subtype)
   else:
       retval = MIMEBase(maintype, subtype)
       retval.set_payload(fd.read())
       Encoders.encode_base64(retval)
   retval.add_header("Content-Disposition", "attachment",
           filename = filename)
   fd.close()
   return retval


today = datetime.date.today()
yesterday = today - datetime.timedelta(days=1)

message = """Hello,

today: %s

yesterday: %s

""" %(today, yesterday)

msg = MIMEMultipart()
msg["To"] = "foo@example.com, bar@example.com"
msg["From"] = "foo_bar@example.com"
msg["Subject"] = "MySubject"
msg["Date"] = Utils.formatdate(localtime = 1)
msg["Message-ID"] = Utils.make_msgid()

body = MIMEText(message, _subtype="plain")
msg.attach(body)
for filename in sys.argv[1:]:
   msg.attach(attachment(filename))
#print msg.as_string()

# Send mail
smtp = smtplib.SMTP()
#smtp.set_debuglevel(1)
smtp.connect("mail.example.com")
smtp.ehlo()
#smtp.login("user", "password")
To = re.split(r", *", msg["To"])
smtp.sendmail(msg["From"], To, msg.as_string())
smtp.quit()</pre>
