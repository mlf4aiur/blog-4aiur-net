Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/04/some-command-from-commandlinefu/
Post: 115
Title: 摘自www.commandlinefu.com的一些命令
Slug: some-command-from-commandlinefu
Postformat: standard
Keywords: shell
Status: publish
Date: 2010-04-06 11:09:55 +0800
Pings: On
Comments: On
Category: Shell

**摘自www.commandlinefu.com的一些命令**

<pre lang="bash"># change to the previous working directory
$ cd -

# quickly backup or copy a file with bash
$ cp filename{,.bak}

# mtr, better than traceroute and ping combined
# mtr combines the functionality of the traceroute and ping programs in a single network diagnostic tool.
# As mtr starts, it investigates the network connection between the host mtr runs on and HOSTNAME. by sending packets with purposly low TTLs. It continues to send packets with low TTL, noting the response time of the intervening routers. This allows mtr to print the response percentage and response times of the internet route to HOSTNAME. A sudden increase in packetloss or response time is often an indication of a bad (or simply over‐loaded) link.
$ mtr google.com

# Salvage a borked terminal
# If you bork your terminal by sending binary data to STDOUT or similar, you can get your terminal back using this command rather than killing and restarting the session. Note that you often won't be able to see the characters as you type them.
$ reset

# Empty a file
# For when you want to flush all content from a file without removing it (hat-tip to Marc Kilgus).
$ > file.txt</pre>
