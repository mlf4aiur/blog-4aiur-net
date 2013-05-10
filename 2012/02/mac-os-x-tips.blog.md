Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2012/02/mac-os-x-tips/
Post: 673
Title: Mac OS X 贴士汇聚
Slug: mac-os-x-tips
Postformat: standard
Keywords: MacOSX, Tips
Status: publish
Date: 2012-02-10 13:27:23 +0800
Pings: On
Comments: On
Category: MacOSX

# Mac OS X tips

### Find & Scan Wireless Networks from the Command Line in Mac OS X

    sudo ln -s /System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport /usr/sbin/airport
    airport -s

### Reindexing Spotlight from the Command Line
Reindexing Spotlight from the command line is done with the mdutil tool, first launch Terminal and then type:

    sudo mdutil -E /

This will reindex every mounted volume on the Mac, including hard drives, disk images, external drives, etc. Specific drives can be chosen by pointing to them in /Volumes/, to only rebuild the primary Macintosh HD:

    sudo mdutil -E /Volumes/Macintosh\ HD/

To reindex an external drive named “External” the command would be:

    sudo mdutil -E /Volumes/External/

Use of the mdutil command will spin up mds and mdworker processes as Spotlight goes to work.

Individually Reindexing Selected Files
In rare cases, Spotlight can miss a file during index, so rather than reindex an entire drive you can also manually add an individual file to the search index with the mdimport command:

    mdimport /path/to/file

The mdimport command can be used on directories as well.

### Convert a DMG file to ISO

    hdiutil convert /path/imagefile.dmg -format UDTO -o /path/convertedimage.iso

### Convert an ISO file to DMG format

    hdiutil convert /path/imagefile.iso -format UDRW -o /path/convertedimage.dmg



### Set a Zip Password in Mac OS X
    zip -e archivename.zip filetoprotect.txt
    unzip filename.zip

### How to Remove Apps from Launchpad in Mac OS X
1. Using Launchpad -- Mac App Store apps only   
Hold down the Option key, and once the icons start jiggling click the “X” shown in the corner of icons that you want to delete. This removes the app from Launchpad, and does not uninstall them, but this is limited to apps installed from the Mac App Store. If you want to remove an app not installed through the Mac App Store, you have to use the method below:
    
2. Using the Terminal -- removes any application   
Launch the Terminal and enter the following command, replacing “APPNAME” with the name of the application you want to remove from Launchpad:

run command:

    sqlite3 ~/Library/Application\ Support/Dock/*.db "DELETE from apps WHERE title='APPNAME';" && killall Dock

### files system usage
    sudo fs_usage -f filesys | grep -vE 'iTerm|X11|Xquartz|grep|syslogd'

### Mac OS X 系统更新升级包下载后的存储位置，避免多台苹果电脑重复下载
打开 Finder，在顶部菜单栏点击 “前往”，在下拉菜单里选择 “前往文件夹…”，粘入下面这个路径，回车。   
看到苹果电脑 Mac OS X 系统更新升级包了吧。   
/library/updates

**注意，上面这些操作的前提是：下载完系统升级包后别立刻安装，否则安装、重启后，Mac 会自动删除它们。**

### Stop Lion from Re-Opening Old Windows with Command+Option+Q
    chflags nohidden ~/Library/
    defaults write com.macromates.textmate NSQuitAlwaysKeepsWindows -bool NO

### Access and Use Emoji in Mac OS X Lion
From almost any Mac OS X Lion app, select the “Edit” menu and pull down to “Special Characters”, or hit Command+Option+T

### Turn web proxy on/off in Mac OS X Terminal
To turn the proxy off, use:

    alias proxyon='networksetup -setwebproxystate Airport on; networksetup -setsecurewebproxystate Airport on'
    alias proxyoff='networksetup -setwebproxystate Airport off; networksetup -setsecurewebproxystate Airport off'

You may need change Airport to the appropriate network service. To list the available network services, use this command:

    networksetup -listallnetworkservices

### Get CPU Info via Command Line in Mac OS X
    system_profiler | grep Processor
    sysctl -n machdep.cpu.brand_string

### Change the terminal window title
    name=`hostname`
    echo -n -e "\033]0;$name\007"
    title () { title=$1; echo -n -e "\033]0;$title\007" }

### Convert a Text File to RTF, HTML, DOC, and more via Command Line
    textutil -convert filetype filename
    textutil -cat rtf file1.txt file2.txt file3.txt -output combinedFiles.rtf

### Renewing a DHCP lease
    sudo ipconfig set en0 DHCP
    sudo ifconfig en0 down ; sudo ifconfig en0 up

### easily strace all your apache processes
    ps -C apache o pid= | sed 's/^/-p /' | xargs strace
    ps auxw | grep sbin/apache | awk '{print"-p " $2}' | xargs strace
    ps auxw|grep bin/apache | awk '{print $2":"$10}'|awk -F ':' ' {print "-p "$1}'|xargs strace

### a simple interactive tool to convert Simplified Chinese (typed by pinyin) to Traditional Chinese 简繁中文转换
    echo "Simplied Chinese:"; while read -r line; do echo "Traditional Chinese:"; echo $line | iconv -f utf8 -t gb2312 | iconv -f gb2312 -t big5 | iconv -f big5 -t utf8; done

### Changing Hostname on Mac OS X
    sudo scutil --set HostName MY_NEW_HOSTNAME

### keep ssh conniction alive
    /private/etc/ssh_config
        ServerAliveInterval 60
        ServerAliveCountMax 3

    /private/etc/sshd_config
        ClientAliveInterval 60
        ClientAliveCountMax 3

### Flush DNS cache 
    dscacheutil -flushcache

### Get DNS Server IP Addresses from the Command Line in Mac OS X
    networksetup -getdnsservers airport


### Command line MP3 player in Mac OS X
    afplay audiofile.mp3

### Take a Screenshot with iPhone
Taking a screenshot using an iPhone, iPod touch, or iPad is really easy. All you need to do is:
    
> Hold down the Power button and Home button simultaneously

> When the screen flashes, a screenshot has taken

### 隐藏文件的不同方式
有如其他Unix，你可以在文件名前加上"."来使其隐藏，例如 /.vol。

这在Finder中是有效的，不过在"ls -a"时却会显示出来。

Mac OS X使用根目录的 .hidden 文件管理需要在Finder中隐藏的文件列表。

同样，HFS+(Mac OS的文件系统)文件和目录可以有一个隐藏属性，通过SetFile命令来设置，`SetFile -a V <filename>`。

你也可以关闭隐藏 属性，通过`SetFile -a v <filename>`。

查看SetFile的man手册了解更多，注意拥有隐藏属性的文件只是在Finder中隐藏，而ls命令仍然可以看到。

### 设置自动代理
设置Network Preference中AirPort中的Automatic Proxy Configuration

选择proxy.pac后restart AirPort。

### 命令行打开关闭网卡命令
    networksetup -setairportpower en1 off
    networksetup -setairportpower en1 on

### Twitter for Mac RT
    defaults write com.twitter.twitter-mac QuoteTweetSyntax -string 'RT @{USERNAME}: {TEXT}'

### Lock the Mac OS X Desktop
1. shift+ctrl+reject
2. via Menu Bar  
Launch “Keychain Access”, open Preferences, Select the checkbox next to “Show Status in Menu Bar”
3. 另外一个方法就是设置Expose属性，在Active Screen Corners中找一个顺手的位置选择Put Display to Sleep，以后鼠标移动到那个角落就可以锁定屏幕了

### 删除菜单栏的图标
我们精彩会发现菜单栏右上角有些图标是我们用不到的，那些可能是系统自带的，也可能是一些安装程序的默认设置，
按住command直接用鼠标把上面的图标从菜单栏拖下来即可。

### How to Remove Icons from the Menu Bar in Mac OS X
holding down the Command key and dragging items out of the menu

### 快速使用Google搜索 
只要是Cocoa程序, 你都可以选择一些文字然后按Shift+Command+L快速以Google搜索。（在Safari中试试看） 

### quickly access System Preferences
If you want to quickly access the Mac OS X System Preferences,
you can do so by holding down the Option key and then hitting various function keys.
Option+Brightness pulls up the Display preference pane, Option+Expose brings up the Expose preferences,
Option+Volume controls bring up the Sound preferences, and so on.

### Mac OS X keyboard shortcuts

* 关机、重启、休眠
    * Command+Control+Eject - 重启
    * Control+Optionion+苹果键+Eject - 关机
    * Optionion+苹果键+Eject - 休眠
    * Control+Eject - 提示关机、重启或者休眠

* Startup keyboard shortcuts
    * Option - Display all bootable volumes (Startup Manager)
    * Shift - Perform Safe Boot (start up in Safe Mode)
    * C - Start from a bootable disc (DVD, CD)
    * T - Start in FireWire target disk mode
    * N - Start from NetBoot server
    * X - Force Mac OS X startup (if non-Mac OS X startup volumes are present)
    * Command-V - Start in Verbose Mode
    * Command-S - Start in Single User Mode

* 截屏
    * Command (⌘)-Shift-3 - 抓取全部屏幕
    * Command (⌘)-Shift-4 - 抓取部分屏幕

* Optionion+Command (⌘)+D - 隐藏DOCK
* Optionion+Command (⌘)+Escape - 强制退出应用程序
* shift＋alt＋音量调控键 - 微调音量。
* 文本编辑选中不连贯部分
    快捷键：Shift-Command-拖拽  
    在文本编辑中，有一个非常有用的技巧，可以让你同时选中文本中多个不连接的部分  
    实现这个功能其实很简单，只要在选择文本的时候按住Shift-Command进行拖拽就可以了。  
    通过这个功能，我们可以对文本中多个不连接的部分同时进行操作，大大节省了我们的时间。  
    通过试验，这个功能不仅仅只在“文本编辑”应用中可以使用，在Pages，Smultron等文本编辑器中都有这个功能。  
    所以，下次你用在编辑文本的时候，可以试试这个小功能，看它能不能帮助你提高效率 :-)

### References

* <http://osxdaily.com>
* <http://support.apple.com/kb/HT1343?viewlocale=en_US&locale=en_US>
