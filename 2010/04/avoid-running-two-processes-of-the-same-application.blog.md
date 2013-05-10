Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/04/avoid-running-two-processes-of-the-same-application/
Post: 106
Title: 避免相同的程序重复运行
Slug: avoid-running-two-processes-of-the-same-application
Postformat: standard
Keywords: shell
Status: publish
Date: 2010-04-06 11:02:21 +0800
Pings: On
Comments: On
Category: Shell

**避免相同的程序重复运行的小函数**

逻辑如下：

1. 检查pid文件是否存在，不存在则直接运行程序
2. 如果pid文件存在，则检查进程中是否存在这个pid
    1. 进程存在此pid则退出
    2. 进程中不存在这一pid，则删除pid文件，程序继续运行

缺点：没有对进程名称进行对比

<pre lang="bash"># Initialize variables
app_path="$(dirname $0)"
pid="${app_path}/run.pid"
# Function
TestPID () {
  if [ -f ${pid} ] ; then
     if ps -p `cat ${pid}` >/dev/null ; then
        echo "`basename $0` is running!  main process id = $(cat ${pid})"
        ps -p `cat ${pid}` -o cmd=
        exit 1
     else
        rm -f $pid
        echo $$ > ${pid}
     fi
  else
     echo $$ > ${pid}
  fi
}</pre>
