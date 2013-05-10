Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/09/macosx-use-opensnoop-trace-progress/
Post: 277
Title: MacOSX使用opensnoop跟踪进程使用情况
Slug: macosx-use-opensnoop-trace-progress
Postformat: standard
Keywords: MacOSX
Status: publish
Date: 2010-09-28 10:09:52 +0800
Pings: On
Comments: On
Category: MacOSX

**MacOSX使用opensnoop跟踪进程使用情况**

opensnoop参数如下：

<pre lang="bash">$ opensnoop -hUSAGE: opensnoop [-a|-A|-ceghsvxZ] [-f pathname]
                 [-n name] [-p PID]
       opensnoop                # default output
                -a              # print most data
                -A              # dump all data, space delimited
                -c              # print cwd of process
                -e              # print errno value
                -g              # print command arguments
                -s              # print start time, us
                -v              # print start time, string
                -x              # only print failed opens
                -Z              # print zonename
                -f pathname	# pathname name to snoop
                -n name		# process name to snoop
                -p PID		# process ID to snoop
  eg,
       opensnoop -v             # human readable timestamps
       opensnoop -e             # see error codes
       opensnoop -f /etc/motd   # snoop this file only</pre>
