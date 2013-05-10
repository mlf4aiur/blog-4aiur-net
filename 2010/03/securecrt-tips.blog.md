Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/03/securecrt-tips/
Post: 16
Title: SecureCRT的使用技巧
Slug: securecrt-tips
Postformat: standard
Keywords: SecureCRT
Status: publish
Date: 2010-03-31 16:18:55 +0800
Pings: On
Comments: On
Category: SysAdmin

**SecureCRT的使用技巧**

1、由于网络条件问题，SecureCRT经常会在不使用的时候开服务器的连接。

用下面的方法设置会很大程度上解决上面的问题。（以SecureCRT Version 5.1.0为例）

* 打开SecureCRT选择Options的General
* 点Default Session之后选择Edit Default Settions
* 再选择Terminal,把Send protocol NO_OP 选上之后全部点ok即可。

注：
同样利用screen命令也可以
使用方法：
SSH登录后执行  
`# screen -S freebsd`  
一旦断线可使用  
`# screen -x freebsd`  
来恢复  

2、密钥的使用

使用密钥便于连接与管理服务器，也极大的增加了系统的安全性。  
个人建议禁止root直接登陆服务器，禁用密码直接登陆服务器，安装密钥时设置密码。  
具体使用方法可以到网上搜集资料，这里不再多说。  

3、与服务器之间的文件传输

设置连接的属性  
选中一连接后鼠标右键点Properties，选择第一项Connection，把Protocol里面的Terminal设置为SSH2，File设置为SFTP（注需要事先安装SecureFX，否则没有次选项）  
进入一个连接后点File菜单，点Connect SFTP Tab（快捷键为alt+p）,会打开SFTP窗口，可以使用命令传输文件。  
命令举例：

* lcd 进入本地目录
* lpwd 显示本地目录路径
* cd 进入服务器本身的目录
* pwd 显示服务器本身的路径
* get 下载服务器文件至本地目录
* put 上传本地文件至服务器

SecureFX是一个使用sftp协议的客户端程序，使用方法与其他ftp客户端类似。

4、使用鼠标键的左键复制与中键粘贴

在SecureCRT的options里面的Global Opations Terminal Mouse选项中。


SecureCRT的多窗口同时操作方法：

* 菜单栏view->"Chat Window"
* 右键点击SecureCRT下方的"Chat Window"栏，选中“Send chat to all tabs”
* 之后在"Chat Window"栏中输入的命令即可发送到同一SecureCRT的多个tables里面。

UNIX-LIKE OS使用ssh命令连接其他ssh server时idle时间过长的话，会出现断开连接的问题。

可以通过配置ssh_config这个配置文件来解决。  
加入下面两条配置来让ssh自动发送NOOP信息，保持ssh的连接。  

* ServerAliveInterval 60
* ServerAliveCountMax 3

<pre>ServerAliveInterval
Sets a timeout interval in seconds after which if no data has been received from the
server, ssh will send a message through the encrypted channel to request a response
from the server.  The default is 0, indicating that these messages will not be sent
to the server.  This option applies to protocol version 2 only.

ServerAliveCountMax
Sets the number of server alive messages (see above) which may be sent without ssh
receiving any messages back from the server.  If this threshold is reached while
server alive messages are being sent, ssh will disconnect from the server, terminat-
ing the session.  It is important to note that the use of server alive messages is
very different from TCPKeepAlive (below).  The server alive messages are sent
through the encrypted channel and therefore will not be spoofable.  The TCP
keepalive option enabled by TCPKeepAlive is spoofable.  The server alive mechanism
is valuable when the client or server depend on knowing when a connection has become
inactive.

The default value is 3.  If, for example, ServerAliveInterval (above) is set to 15,
and ServerAliveCountMax is left at the default, if the server becomes unresponsive
ssh will disconnect after approximately 45 seconds.</pre>
