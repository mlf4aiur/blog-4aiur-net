Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2011/03/cacti-configuration-guide/
Post: 354
Title: Cacti配置流程
Slug: cacti-configuration-guide
Postformat: standard
Keywords: Cacti, monitor
Status: publish
Date: 2011-03-16 12:25:52 +0800
Pings: On
Comments: On
Category: SysAdmin

# Cacti配置流程

Cacti是一个简单直观的监控工具，后台使用rrdtool记录监控数据，虽然功能较少，但是图形显示效果比较好看、直观、配置也比较方便，当需要有复杂的监控需求时，可以使用zabbix或者nagios来做。

这里主要讲的是Cacti两方面的配置，一个是添加设备，另外一个是配置权限

1. 添加监控设备
    1. 首先需要在被监控的设备上安装与配置snmp agent，Linux平台请参考[Linux安装与配置Snmpd](http://blog.4aiur.net/2011/03/345)，OpenSolaris参考[OpenSolaris net-snmp install script](http://blog.4aiur.net/2010/12/303)，windows安装配置snmp agent方法  
    > 检查是否存在SNMP Service，需要将此服务启动。我的电脑-管理-服务-SNMP Ssrvice.  
    > 单击属性-安全，添加发送身份验证陷阱。添加在cacti中的SNMP Community，在下面添加监控端的IP地址  
    > 如果没有此服务，通过控制面板-添加组件-管理和监视工具-简单网络管理协议。

    2. 添加设备  
    登陆Cacti后点击面板左侧的devices后点击右侧面板的Add，之后填写Description, Hostname, Host Template(Linux与Solaris使用ucd/net SNMP Host，Windows使用windows2000/xp Host), 其他部分可以使用自己的值来填写"SNMP Community"或者保持不变。配置完成后点击Create增加此设备。
    在创建完毕后出现的页面中点下Query Verbose 来测试下snmp数据抓取是否正常。  
    <img src="http://blog.4aiur.net/wp-content/uploads/2011/03/cacti_data_query_debug1.png" alt="Cacti Data Query Debug" height="125" width="535">
    3. 添加图像  
    点击页面中的"Create Graphs for this Host"  
    选中所有复选框后点击"Create"，完成生成图像的设置  
    4. 设置图像数  
    点击面板左侧的"Graph Trees" --> "Add"，输入name后"Create" --> "Save"  
    5. 把设备添加到心的图像树中  
    在"Devices"中选中刚加入的设备选择  
    <img src="http://blog.4aiur.net/wp-content/uploads/2011/03/cacti_place_on_a_tree1.png" alt="Cacti Place on a Tree" height="99" width="414">  
    "Place on a Tree" --> "Go" --> "Continue"，到这里设备的配置已经完成，可以点击导航栏的graphs查看新增加设备的各种监控图像，下面是我Cacti监控数据的两个截图  
    <img src="http://blog.4aiur.net/wp-content/uploads/2011/03/linux_traffic.png" alt="Linux Traffic" height="270" width="615">  
    <img src="http://blog.4aiur.net/wp-content/uploads/2011/03/linux_load_average.png" alt="Linux Load Average" height="301" width="629">  


2. 权限配置
    1. 增加新用户  
    "User Management" --> "Add"，填写相应项"User Name", "Full Name", "Password", "Enabled Determines if user is able to login."这里选中"Enabled"  
    权限规则"Realm Permissions"处选中"View Graphs", "Export Data"即可  
    点击"Create"后进行图像的权限设置。  
    <img src="http://blog.4aiur.net/wp-content/uploads/2011/03/cacti_tree_permission.png" alt="Cacti Tree Permission" height="188" width="503">
    2. 权限设置  
    Tree Permissions中"Default Policy"使用"Deny"，并增加新增的图像树如下图，之后"Save"  
    把"Graph Permissions (By Graph)", "Graph Permissions (By Device)", "Graph Permissions (By Graph Template)"后面的"Policy"修改为"Allow"，之后"Save"。  
    到这里Cacti的配置已经完成"Logout"后使用新加的用户登陆即可查看设备的监控信息。
