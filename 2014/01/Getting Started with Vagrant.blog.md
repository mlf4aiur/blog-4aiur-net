Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2014/01/getting-started-with-vagrant/
Post: 967
Title: Getting Started with Vagrant
Slug: getting-started-with-vagrant
Postformat: standard
Keywords: Chef, Vagrnat
Status: publish
Date: 2014-01-12 18:18:43 +0800
Pings: On
Comments: On
Category: SysAdmin

Getting Started with Vagrant
============================

Installation
------------

Install VirtualBox or VMware first, and download the latest from [Vagrant download page](http://www.vagrantup.com/downloads.html).

Launch a instance
-----------------

    cd ~/.vagrant.d/tmp/
    wget http://developer.nrel.gov/downloads/vagrant-boxes/CentOS-6.4-x86_64-v20130731.box
    vagrant box add CentOS-6.4-x86_64 CentOS-6.4-x86_64-v20130731.box
    mkdir -p ~/.vagrant.d/init/myvm
    cd ~/.vagrant.d/init/myvm
    vagrant init CentOS-6.4-x86_64
    vagrant up
    vagrant ssh

Provisioning
------------

    # install rvm
    \curl -sSL https://get.rvm.io | bash -s stable
    source "$HOME/.rvm/scripts/rvm
    gem install chef
    mkdir -p ~/.vagrant.d/init/my-recipes/cookbooks
    cd ~/.vagrant.d/init/my-recipes/cookbooks
    for cookbook in yum mysql apache2
    do
        knife cookbook site download $cookbook
    done
    # untar the cookbook tar file

**`Vagrantfile`**

    config.vm.provision :chef_solo do |chef|
      chef.cookbooks_path = "../my-recipes/cookbooks"
    
      chef.add_recipe "yum"
      chef.add_recipe "yum::epel"
      chef.add_recipe "mysql"
      chef.add_recipe "apache2"
    
      # You may also specify custom JSON attributes:
      chef.json.merge!({
        :mysql => {
          "server_root_password" => "yourmysqlpassword",
          "server_repl_password" => "yourmysqlpassword",
          "server_debian_password" => "yourmysqlpassword"
        },
        :apache2 => {
          "listen_address" => "0.0.0.0"
        }
      })
    end

    config.vm.provision :shell, :path => "provision.sh"

**`provision.sh`**

    #!/usr/bin/env bash
    
    sudo iptables -I INPUT -p tcp --dport 80 -j ACCEPT
    sudo iptables -I INPUT -p tcp --dport 8080 -j ACCEPT
    sudo yum -y update

**Run a specific provisioner**

    vagrant provision --provision-with chef_solo

**Run `chef-solo` on VM**

    sudo chef-solo -c /tmp/vagrant-chef-1/solo.rb -j /vagrant/chef_solo.json

**`/vagrant/chef_solo.json`**

    {
      "mysql": {
        "server_root_password": "yourmysqlpassword",
        "server_repl_password": "yourmysqlpassword",
        "server_debian_password": "yourmysqlpassword"
      },
      "apache2": {
        "listen_address": "0.0.0.0"
      },
      "run_list": [
        "recipe[yum]",
        "recipe[yum::epel]",
        "recipe[mysql]",
        "recipe[apache2]"
      ]
    }

Packaging
---------

**Creating the `Vagrantfile`**

Name the file `Vagrantfile.pkg`

    Vagrant::Config.run do |config|
      # Forward apache
      config.vm.forward_port 80, 8080
    end

**Packaging the Project**

    $ vagrant package --vagrantfile Vagrantfile.pkg

**Distributing the Box**

    $ vagrant box add my_box /path/to/the/package.box
    $ vagrant init my_box
    $ vagrant up

Troubleshooting
---------------

**Enable debug:**

    VAGRANT_LOG=info vagrant up
    set VAGRANT_LOG=info
    vagrant up
    VAGRANT_LOG=debug vagrant provision

**Insecure world writable dir /Applications in PATH, mode 040777**

    Vagrant::Errors::VMBootBadState: The guest machine entered an invalid state while waiting for it
    to boot. Valid states are 'starting, running'. The machine is in the
    'poweroff' state. Please verify everything is configured
    properly and try again.
    
    If the provider you're using has a GUI that comes with it,
    it is often helpful to open that and watch the machine, since the
    GUI often has more helpful error messages than Vagrant can retrieve.
    For example, if you're using VirtualBox, run `vagrant up` while the
    VirtualBox GUI is open.

Run this to fix it:

    sudo chmod 755 /Applications/

**Mount error on v-root: /vagrant**

When you start up your guest, you may get the following message unexpectedly:

    [default] -- v-root: /vagrant
    The following SSH command responded with a non-zero exit status.
    Vagrant assumes that this means the command failed!

    mount -t vboxsf -o uid=`id -u vagrant`,gid=`id -g vagrant` v-root /vagrant

This is usually a result of the guest's package manager upgrading the kernel
without rebuilding the VirtualBox Guest Additions.
To double-check that this is the issue,
connect to the guest and issue the following command:

    lsmod | grep vboxsf

If that command does not return any output, it means that the VirtualBox
Guest Additions are not loaded. If the VirtualBox Guest Additions were
previously installed on the machine, you will more than likely be able to
rebuild them for the new kernel through the `vboxadd initscript`, like so:

    sudo /etc/init.d/vboxadd setup

**Upgrade VirtualBox Guest Additions <https://gist.github.com/fernandoaleman/5083680>**

    [default] The guest additions on this VM do not match the installed version of
    VirtualBox! In most cases this is fine, but in rare cases it can
    cause things such as shared folders to not work properly. If you see
    shared folder errors, please update the guest additions within the
    virtual machine and reload your VM.
    
    Guest Additions Version: 4.2.8
    VirtualBox Version: 4.3

Solution:  
Attach VBoxGuestAdditions.iso(can be found /Applications/VirtualBox.app/Contents/MacOS/VBoxGuestAdditions.iso) in VirtualBox manually.

**For Ubuntu should run next commands:**

    sudo apt-get update -q
    sudo apt-get install -q -y dkms
    # Install linux-headers
    sudo apt-get install -q -y linux-headers-3.2.0-23-generic
    sudo mount -o loop,ro /dev/sr0 /mnt

**For CentOS just need to run next command:**

    sudo mount -t iso9660 -o loop,ro /dev/cdrom /mnt/

**Upgrade VirtualBox Guest Additions:**

    sudo sh /mnt/VBoxLinuxAdditions.run --nox11
    exit
    vagrant reload

**Fix Vagrant Box Hanging at Boot(grub menu)**

Use VirtualBox to boot this VM, and run command: `sudo update-grub`

Related links:
--------------

* [Vagrant Docs V1](http://docs-v1.vagrantup.com/v1/docs/getting-started/index.html)
* [Vagrant Docs V2](http://docs.vagrantup.com/v2/getting-started/index.html)
* [Vagrantbox.es](http://www.vagrantbox.es/)
* <http://rove.io>
