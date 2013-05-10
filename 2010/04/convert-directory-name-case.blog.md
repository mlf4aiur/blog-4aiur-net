Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/04/convert-directory-name-case/
Post: 123
Title: 目录名大小写转换
Slug: convert-directory-name-case
Postformat: standard
Keywords: shell
Status: publish
Date: 2010-04-06 11:21:06 +0800
Pings: On
Comments: On
Category: Shell

**使用find把目录名修改为大写**

<pre lang="bash">find . -type d | sort -r |\
while read name
do
   echo "mv $name ${name%/*}/`echo ${name##*/} | tr '[:lower:]' '[:upper:]'`"
   mv $name ${name%/*}/`echo ${name##*/} | tr '[:lower:]' '[:upper:]'`
done</pre>

**使用递归方式把目录名修改为小写**

<pre lang="bash">#!/bin/bash
# set -x

tolower () {
   ls | while read name
   do
       if [ -d $name ] ; then
           new_name=`echo $name | tr "[:upper:]" "[:lower:]"`
           if [[ $name != $new_name ]] ; then
               mv $name $new_name
           fi
           cd $new_name
           tolower
       fi
   done
}
tolower</pre>
