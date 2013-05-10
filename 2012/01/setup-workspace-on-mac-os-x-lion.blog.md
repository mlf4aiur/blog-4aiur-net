Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2012/01/setup-workspace-on-mac-os-x-lion/
Post: 667
Title: Setup Workspace On Mac OS X Lion
Slug: setup-workspace-on-mac-os-x-lion
Postformat: standard
Keywords: MacOSX
Status: publish
Date: 2012-01-19 14:52:18 +0800
Pings: On
Comments: On
Category: MacOSX

# Setup Workspace On Mac OS X Lion

### Software Update
Click Apple () menu, choose **Software Update**

### Change System Preferences
Click Apple () menu, choose **System Preferences**

1. Trackpad  
Tap to click  
Scroll direction: natural
2. Mouse  
Adjust Tracking Speed to max
3. Keyboard  
Keyboard Shortcuts  
Full Keyboard Access: In windows and dialogs, press Tab to move keybiard focus between: --> select "All controls"
3. Dock  
Magnification  
Change Size and Magnification  
Automatically hide and show the Dock
4. Mission Control  
Show Dashboard as a space  
Change Show Dashboard shortcut to F4
5. Language & Text  
Input Sources --> Chinese - Simplified --> Pinyin - Simplified  
Change Input source shortcuts to ^Space
6. Sound  
Remove "Show volume in menu bar"
7. Security & Privacy  
Choose "Disable restarting to Safari when screen is locked"
8. Sharing  
Change your "Computer Name"
9. Time Machine  
Remove "Show Time Machine status in menu bar"

### Dashboard Widgets
* Dictionary  
For Chinese:  
Install [DictUnifier](http://code.google.com/p/mac-dictionary-kit/downloads/list), and import dictionary: stardict-langdao-ec-gb-2.4.2.tar.gz, and drag "stardict-langdao-ec-gb-2.4.2.tar.bz2" into DictUnifier.  
Now setup Dictionary Preferences, change the dicionary order.
* iSTAT NANO  
An advanced system monitor in a tiny package. iStat nano is a stunning system monitor widget with beautifly animated menus and transitions.  
[Download Page](http://www.apple.com/downloads/dashboard/status/istatnano.html)  
* To Do  
A lightweight and fast widget to manage tasks. Thanks to Mac OS X Leopard it integrates with iCal and Mail. The big advantage: to manage your tasks you don’t have to leave these applications open.  
[Download Page](http://www.apple.com/downloads/dashboard/email_messaging/todo.html)  

### Adjust System
1. Finder  
View --> Show Path Bar, Show Status Bar  
Press "CMD + ," setup Finder Preferences.  
General --> "New Finder windows show:" --> User's Home Directory  
Sidebar --> Add your home dir, and remove "All My Files", "Music", "Pictures", "Hard disks"...  
Advanced --> unselect "Empty Trash securely", When performing a search --> Search the Current Folder
2. Safari  
Goto [YouTuBe](http://www.youtube.com/), Click "Download Adobe Flash Player from Adobe", and install it.  
View --> Show Path Bar, Show Status Bar  
Press "CMD + ," setup Finder Preferences.  
Tabs --> Open pages in tabs instead of windows: Always  
Extensions --> Get Extensions --> Install "AdBlock", "ClickToFlash"  
Advanced --> "Never use font sizes smaller than: 12"
3. Mail  
Add new account, and add yourself rules in Mail's Preferences.
4. sudoers  
`$ sudo vi visudo`  
`# Uncomment to allow people in group wheel to run all commands`  
`%wheel    ALL=(ALL) ALL`
5. 

### Install Software
* App Store
* Manually

#### From App Store
**Sign in**

Using Apple ID sign in, or create new Apple ID.  

**Install Software**

* Xcode
* Caffeine  
* Skitch
* Evernote
* TextWrangler
* Opus Domini
* Window Tidy
* Twitter
* The Unarchiver
* MindNode
* SketchBook Express
* 1Password

#### Manually
* Adium  
Adium is a free instant messaging application for Mac OS X that can connect to AIM, MSN, Jabber, Yahoo, and more.  
[WebSite](http://adium.im)
* AppCleaner  
AppCleaner is a small application which allows you to thoroughly uninstall unwanted apps.  
[WebSite](http://www.freemacsoft.net/)
* GitHub  
GitHub is the best way to collaborate with others. Fork, send pull requests and manage all your public and private git repositories.  
[Download page](http://mac.github.com/)
* iTerm2  
iTerm2 is a replacement for Terminal and the successor to iTerm. It works on Macs with Leopard, Snow Leopard, or Lion. Its focus is on performance, internationalization, and supporting innovative features that make your life better.  
[Download page](http://code.google.com/p/iterm2/downloads/list)  
* MPlayer OSX Extended  
MPlayer OSX Extended is the future of MPlayer OSX. Leveraging the power of the MPlayer and FFmpeg open source projects, MPlayer OSX Extended aims to deliver a powerful, functional and no frills video player for OSX.  
[Download page](http://www.mplayerosx.ch/#downloads)
* Sequel Pro  
Sequel Pro is a fast, easy-to-use Mac database management application for working with MySQL databases.  
[Download page](http://www.sequelpro.com/)
* FireFox  
[Download page](http://www.mozilla.org/en-US/products/download.html)
* TextMate2  
TextMate brings Apple's approach to operating systems into the world of text editors. By bridging UNIX underpinnings and GUI, TextMate cherry-picks the best of both worlds to the benefit of expert scripters and novice users alike.  
[WebSite](http://macromates.com/)
* Sublime Text 2  
Sublime Text is a sophisticated text editor for code, html and prose. You'll love the slick user interface and extraordinary features.  
[WebSite](http://macromates.com/)
* Skype  
[WebSite](http://www.skype.com/)
* Chicken of the VNC  
Chicken of the VNC is a VNC client for Mac OS X. A VNC client allows one to display and interact with a remote computer screen. In other words, you can use Chicken of the VNC to interact with a remote computer as though it's right next to you.  
[Download page](http://sourceforge.net/projects/cotvnc/)
* MacPorts  
The MacPorts Project is an open-source community initiative to design an easy-to-use system for compiling, installing, and upgrading either command-line, X11 or Aqua based open-source software on the Mac OS X operating system.  
[Download page](http://www.macports.org/install.php)

#### Command Line
**vim**

    curl -O "ftp://ftp.vim.org/pub/vim/unix/vim-7.3.tar.bz2"
    tar jxf vim-7.3.tar.bz2
    cd vim73
    ./configure --enable-pythoninterp --enable-rubyinterp --enable-multibyte
    make && sudo make install

**port**

    sudo port selfupdate
    sudo port -c install wget@ssl
    sudo port -c install axel
    sudo port -c install gsed
    sudo port -c install nmap

**pip**

    # Install pip
    sudo easy_install-2.7 pip
    # Install python libraries
    # ipython - IPython: Productive Interactive Computing
    sudo pip-2.7 install ipython
    # Mercurial - Fast scalable distributed SCM (revision control, version control) system
    sudo pip-2.7 install Mercurial
    # pep8 - Python style guide checker
    sudo pip-2.7 install pep8
    # flake8 - code checking using pep8 and pyflakes
    sudo pip-2.7 install flake8
    # pyflakes - passive checker of Python programs
    sudo pip-2.7 install pyflakes
    # ropevim- A vim plugin for using rope python refactoring library
    sudo pip-2.7 install ropevim
    # paramiko                  - SSH2 protocol library
    sudo pip-2.7 install paramiko

**RVM**

    bash -s stable < <(curl -s https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvm-installer )
    # Load RVM by appending the rvm function sourcing to your .bash_profile:
    echo '[[ -s "$HOME/.rvm/scripts/rvm" ]] && . "$HOME/.rvm/scripts/rvm" # Load RVM function' >> ~/.bash_profile

**.bash_profile**

    cat >> ~/.bash_profile <<\EOF
    alias rm='rm -i'
    alias cp='cp -i'
    alias mv='mv -i'
    alias ls='ls -G'
    alias ll='ls -lG'
    alias vi='/usr/local/bin/vim'
    alias grep='grep --color'
    alias Github='cd ~/Documents/workspace/Github/'
    alias cgiserver="ifconfig | grep --color -o "inet 1[079][^ ]*"; python -m CGIHTTPServer 8000" # cgi_directories ['/cgi-bin', '/htbin']
    alias webshare='share () { port=$1; ifconfig | grep --color -o "inet 1[079][^ ]*"; python -m SimpleHTTPServer ${port:-8000} ;}; share'
    title () { title=$@; echo -n -e "\033]0;${title}\007"; }

    export LSCOLORS=ExGxCxDxCxEgEdAbAgAcAd
    export PS1='[\u@\h \W]\$ '

    export LANGUAGE=en_US.UTF-8
    export LANG=en_US.UTF-8
    export LC_ALL=en_US.UTF-8

    shopt -s histappend
    PROMPT_COMMAND="history -a; "
    HISTTIMEFORMAT="%F %T "
    HISTSIZE=2000
    export HISTTIMEFORMAT
    export PATH=/opt/local/bin:/opt/local/sbin:$PATH
    EOF

### Adjust your Dock
Remove your application not very often, and add your favorite applications into Dock.

### Applications Tips

**MacPorts**

    # Search software
    port search vim
    # Install software
    port install wget@ssl
    sudo port -d selfupdate
    sudo port -v selfupdate
    port outdate
    sudo port -c -u upgrade outdated
    port installed

**pip**

    # search: Search PyPI
    pip search pep8
    #install: Install packages
    pip install pep8
    #uninstall: Uninstall packages
    pip uninstall pep8

**RVM**

    # Display a list of all "known" rubies. NOTE: RVM can install many more Rubies not listed.
    rvm list known
    # Install a version of Ruby (eg 1.9.3):
    rvm install 1.9.3
    # Use the newly installed Ruby:
    rvm use 1.9.3
    # Check this worked correctly:
    ruby -v
    which ruby
    # Optionally, you can set a version of Ruby to use as the default for new shells. Note that this overrides the 'system' ruby:
    rvm use 1.9.3 --default
