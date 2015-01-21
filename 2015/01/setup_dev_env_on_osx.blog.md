Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2015/01/setup-development-environment-on-osx/
Title: Setup development environment on OSX
Slug: setup-development-environment-on-osx
Postformat: standard
Keywords: Docker
Status: publish
Pings: On
Comments: On
Category: MacOSX

Setup development environment on OSX
====================================

**Install Homebrew on your laptop**

    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

**Install some packages you like**

    cat > Caskfile <<\EOF
    # Install Cask
    install caskroom/cask/brew-cask

    # Install Casks
    cask install alfred
    cask install caffeine

    cask install virtualbox
    cask install boot2docker

    cask install google-chrome
    cask install iterm2
    cask install sequel-pro
    EOF

    export HOMEBREW_CASK_OPTS="--appdir=/Applications --caskroom=/etc/Caskroom"
    brew bundle Caskfile

**Initialize virtual machine**

    boot2docker init
    boot2docker start

**A demonstration for build rpm in container**

    cat > Dockerfile <<\EOF
    FROM centos:centos6

    MAINTAINER Kevin Li <mlf4aiur@gmail.com>

    RUN yum clean all
    RUN yum -y update && yum install -y \
        ruby-devel.x86_64 \
        ruby.x86_64 \
        rubygems.noarch \
        gcc.x86_64 \
        rpm-build.x86_64
    RUN gem install fpm --no-rdoc --no-ri

    VOLUME /data
    WORKDIR /data

    CMD ["/bin/bash"]
    EOF

    $(boot2docker shellinit)
    docker build -t mlf4aiur/fpm_rpm .

Put your package into data/ directory and run following command, then you'll get the rpm file in you data directory.

    docker run -it --rm -v `pwd`/data:/data mlf4aiur/fpm_rpm fpm \
        --verbose \
        -f \
        -s dir \
        -t rpm \
        --name hello_world \
        -v 0.9.0 \
        --config-files opt/hello_world/ \
        --exclude "*/*.pyc" \
        --depends python \
        opt/hello_world
