Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/04/concurrent-use-of-xargs/
Post: 127
Title: 利用xargs实现多任务并发
Slug: concurrent-use-of-xargs
Postformat: standard
Keywords: shell
Status: publish
Date: 2010-04-06 11:22:34 +0800
Pings: On
Comments: On
Category: Shell

**利用xargs实现多任务并发**

<pre lang="bash">[4aiur@localhost Temp]$ echo 127.0.0.1 192.168.2.50 192.168.10.108 | xargs -n1 -P2 -I % ping -c 3 %
PING 127.0.0.1 (127.0.0.1): 56 data bytes
64 bytes from 127.0.0.1: icmp_seq=0 ttl=64 time=0.048 ms
PING 192.168.2.50 (192.168.2.50): 56 data bytes
64 bytes from 192.168.2.50: icmp_seq=0 ttl=64 time=0.026 ms
64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.091 ms
64 bytes from 192.168.2.50: icmp_seq=1 ttl=64 time=0.022 ms
64 bytes from 192.168.2.50: icmp_seq=2 ttl=64 time=0.039 ms
64 bytes from 127.0.0.1: icmp_seq=2 ttl=64 time=0.077 ms

--- 192.168.2.50 ping statistics ---
3 packets transmitted, 3 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 0.022/0.029/0.039/0.007 ms

--- 127.0.0.1 ping statistics ---
3 packets transmitted, 3 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 0.048/0.072/0.091/0.018 ms
PING 192.168.10.108 (192.168.10.108): 56 data bytes
64 bytes from 192.168.10.108: icmp_seq=0 ttl=63 time=6.244 ms
64 bytes from 192.168.10.108: icmp_seq=1 ttl=63 time=1.392 ms
64 bytes from 192.168.10.108: icmp_seq=2 ttl=63 time=1.062 ms

--- 192.168.10.108 ping statistics ---
3 packets transmitted, 3 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 1.062/2.899/6.244/2.369 ms
[4aiur@localhost Temp]$</pre>


使用xargs扫描同网段内存活的设备

<pre lang="bash">[4aiur@localhost Temp]$ for ((x=1; x<=255; x++)); do echo 192.168.2.$x; done | xargs -n1 -P 255 -I {} ping -c1 {} 2>/dev/null | awk /ttl/'{print $4}'</pre>
