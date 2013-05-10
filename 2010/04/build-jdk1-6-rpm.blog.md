Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2010/04/build-jdk1-6-rpm/
Post: 146
Title: 制作jdk1.6rpm包
Slug: build-jdk1-6-rpm
Postformat: standard
Keywords: Linux, rpm
Status: publish
Date: 2010-04-06 11:34:14 +0800
Pings: On
Comments: On
Category: Linux

**制作jdk1.6rpm包**

**制作rpm包的几个步骤:**

* 制作Makefile
* 生成jdk压缩包
* 生成build rpm使用的spec文件
* 生成rpm包

1) 制作Makefile

<pre lang="bash">[root@localhost foo]# cat Makefile
# Makefile
all:
	@echo Make jdk-1.6.0_08-fcs
	@echo Java(TM) Platform Standard Edition Development Kitinstall:
	mkdir -p /usr/java/ tar -zxf jdk1.6.0_18.tar.gz -C /usr/java
	cp -pf /etc/profile /etc/profile.save if ! grep JAVA_HOME /etc/profile >/dev/null 2>&1 ; then\
	    echo 'export JAVA_HOME=/usr/java/jdk1.6.0_18/' >> /etc/profile; \ echo 'export PATH=$$PATH:/usr/java/jdk1.6.0_18/bin' >> /etc/profile; \
	    echo 'export CLASSPATH=.:$$JAVA_HOME/lib:$$JRE_HOME/lib:$$CLASSPATH' >> /etc/profile; \ fi

uninstall:
	rm -rf /usr/java/jdk1.6.0_18/

clean:
	# Nope.</pre>

**注意事项Makefile中的命令需要以tab开头，不要使用空格。**

2) 生成jdk压缩包下载jdk安装包后，从bin中提取jdk文件

<pre lang="bash">export MORE=10000
sh jdk-6u18-linux-i586.bin <<\EOF >/dev/null
yes
EOF</pre>

把源文件压缩为jdk1.6.0_18.tar.gzMakefile存放到jdk6u18目录中  
压缩jdk6u18目录为jdk6u18.tar.gz存放jdk6u18.tar.gz到/usr/src/redhat/SOURCES  

3) 生成build rpm使用的spec文件

<pre lang="bash">[root@localhost SPECS]# cat jdk1.6.0_18.spec
# Build RPMSummary: Java(TM) Platform Standard Edition Development Kit

Name: jdk-1.6.0_08-fcs

Version: 1.6.0_08

Release: fcs

Vendor: Sun Microsystems, Inc.

Packager: 4Aiur

License: Sun Microsystems Binary Code License (BCL)

URL: http://java.sun.com/

Group: Development/Tools

Source0: jdk6u18.tar.gz

#Autoreq: 0

%description

The Java Platform Standard Edition Development Kit (JDK) includes boththe runtime environment (Java virtual machine, the Java platform classes
and supporting files) and development tools (compilers, debuggers,tool libraries and other tools).

The JDK is a development environment for building applications, appletsand components that can be deployed with the Java Platform Standard
Edition Runtime Environment.

%prep

tar xzvf $RPM_SOURCE_DIR/jdk6u18.tar.gz -C $RPM_BUILD_DIR/

# Avoid RPM 4.2+'s internal dep generator, it may produce bogus# Provides/Requires here.
%define _use_internal_dependency_generator 0

# this will be our replacement for find_requires%define toplevel_dir jdk6u18
%define our_find_requires %{_builddir}/%{toplevel_dir}/find_requires#%define our_find_requires %{_builddir}/jdk6u18/find_requires

# This prevents aggressive stripping.%define debug_package %{nil}

# Kludge to remove bogus odbc dependenciescat <<EOF >%{our_find_requires}
#!/bin/shecho unixODBC
exec %{__find_requires} | /bin/egrep -v '^(libodbc(inst)?\.so)$'exit 0
EOFchmod +x %{our_find_requires}
%define __find_requires %{our_find_requires}

%build

cd $RPM_BUILD_DIR/jdk6u18

make

%install

cd $RPM_BUILD_DIR/jdk6u18

make install

%clean

rm -fr $RPM_BUILD_DIR/jdk6u18

%files

%defattr(-,root,root)/usr/java/jdk1.6.0_18/

%changelog* Fri Mar 24 2010 -- 4aiur
-- Create paceage

[root@localhost SPECS]#</pre>

**注意事项**

取消odbc依赖关系的两种方法

* spec中加入，禁止rpmbuild自动查找依赖关系 #Autoreq: 0
* 过滤__find_requires中的odbc输出

4) 生成rpm包

<pre lang="bash">[root@localhost SPECS]# rpmbuild -bb jdk1.6.0_18.spec</pre>

5) 其它相关信息：

**获取rpm中的文件**

<pre lang="bash">[root@localhost foo]# rpm2cpio logrotate-1.0-1.i386.rpm > logrotate.cpio
[root@localhost foo]# cpio -idmv logrotate.cpio</pre>

**安装官方jdk的方法**

安装jdk时，设置more读取文件的行数位10000，并使用yes命令自动回答安装程序。

<pre lang="bash">export MORE=10000
sh jdk-6u18-linux-i586.bin <<EOF >/dev/null
yes
EOF</pre>

**rpmbuild相关内容**

* rpmbuild使用的库文件所在目录: /usr/lib/rpm
* 查看rpmbuild的变量
<pre lang="bash">[root@localhost foo]# rpm --eval "%__find_requires"
/usr/lib/rpm/redhat/find-requires</pre>
* rpmbuild中使用的宏文件应位于上一文件的相同目录的macros
<pre lang="bash">[root@localhost foo]# ll /usr/lib/rpm/redhat/macros
-rw-r--r-- 1 root root 8845 Sep 29 07:51 /usr/lib/rpm/redhat/macros</pre>
* 只生成二进制格式的rpm包
<pre lang="bash">[root@localhost foo]# rpmbuild -bb xxx.spec</pre>
* 完全打包
<pre lang="bash">[root@localhost foo]# rpmbuild -ba xxx.spec</pre>

**rpm相关**

* 测试rpm包
<pre lang="bash">[root@localhost foo]# rpm --test -ivh /usr/src/redhat/RPMS/i386/jdk-1.6.0_08-fcs-1.6.0_08-fcs.i386.rpm</pre>
* 查看rpm包信息
<pre lang="bash">[root@localhost foo]# rpm -qip /usr/src/redhat/RPMS/i386/jdk-1.6.0_08-fcs-1.6.0_08-fcs.i386.rpm</pre>
* 查看rpm包文件列表
<pre lang="bash">[root@localhost foo]# rpm -qlp /usr/src/redhat/RPMS/i386/jdk-1.6.0_08-fcs-1.6.0_08-fcs.i386.rpm</pre>

**build rpm的资源**

查找rpm资源的地址： <http://dries.ulyssis.org/rpm/>  
可以在这里找到很多编译rpm包的spec源文件。  
可以借鉴其它的spec文件来编写自己的spec文件
