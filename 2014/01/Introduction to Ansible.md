Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2014/01/introduction-to-ansible/
Post: 970
Title: Introduction to Ansible
Slug: introduction-to-ansible
Postformat: standard
Keywords: Ansible
Status: publish
Date: 2014-01-31 22:03:37 +0800
Pings: On
Comments: On
Category: SysAdmin

Introduction to Ansible
=======================

Installation
------------

I don't want input password every time when I run `ansible`, so prepare ssh key authentication agent for auto login.

    # Generate ssh public private key pair
    ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa
    ssh-agent | head -2 > ~/.ssh/.agent.env
    source ~/.ssh/.agent.env
    ssh-add ~/.ssh/id_rsa

Install Ansible on a isolation environment.

    mkdir ~/Ansible
    cd  ~/Ansible  # or press Escape + .
    sudo easy_install pip
    pip install virtualenv
    virtualenv .venv
    source .venv/bin/activate
    pip install ansible

Setup `autoenv` for load `ansible` environment automatically. 

    git clone git://github.com/kennethreitz/autoenv.git ~/.autoenv
    echo "source ~/Ansible/active_ansible" > .env
    cat > active_ansible <<\EOF
    source ~/Ansible/.venv/bin/activate  # Activeate virtualenv
    source ~/.ssh/.agent.env  # Load ssh-agent env
    export ANSIBLE_HOSTS=~/Ansible/inventory
    export ANSIBLE_HOST_KEY_CHECKING=False  # Set ansible config
    export ANSIBLE_FORKS=20  # Specify ansible parallel processes
    EOF

Usage examples
--------------

**Inventory file**

    cat ~/Ansible/inventory
    [web]
    web1.example.com
    web2.example.com
    
    [db]
    db1.example.com
    db2.example.com

**Run some ad-hoc commands**

    ansible web -m shell -a "uptime"

**File Transfer**

    ansible web -m copy -a "src=foo dest=/tmp/foo"

**Gathering Facts**

    ansible web -m setup

Playbook
--------

**Distribute the public_key to remote hosts with playbook**

    cat > playbook.yaml <<EOF
    ---
    - hosts: all
      vars:
        authorized_keys: ~/.ssh/id_rsa.pub
        ssh_dir: /home/mlf4aiur/.ssh
        owner: mlf4aiur
        group: mlf4aiur
      tasks:
      - name: set attributes of .ssh
        action: file path={{ ssh_dir }} owner={{ owner }} group={{ group }} mode=0700 state=directory
      - name: put public key in ~/.ssh/
        action: copy src={{ authorized_keys }} dest={{ ssh_dir }}/authorized_keys owner={{ owner }} group={{ group }} mode=0600
    EOF

    ansible-playbook playbook.yaml -k -e "ssh_dir=/home/mlf4aiur/.ssh owner=mlf4aiur"

**Write a customize playbook**

Here is a Ansbile playbook example.

Playbook directory structure.

    $ tree
    |-roles
    |---example      # Role name
    |-----files      # Statistic files
    |-----tasks      # Put tasks here
    |-----templates  # Template files
    |-----vars       # Variabals
    
    # roles/example/vars/main.yml
    ---
      authorized_keys: id_rsa.pub
      ssh_dir: /home/mlf4aiur/.ssh
      owner: mlf4aiur
      group: mlf4aiur

    # roles/example/files/id_rsa.pub
    # roles/example/tasks/main.yml
    ---
    - name: set attributes of .ssh
      action: file path={{ ssh_dir }} owner={{ owner }} group={{ group }} mode=0700 state=directory
    - name: put public key in ~/.ssh/
      action: copy src={{ authorized_keys }} dest={{ ssh_dir }}/authorized_keys owner={{ owner }} group={{ group }} mode=0600

Apply this playbook to distribute public_key.

    # distribute_public_key.yml
    ---
    - hosts: all
      gather_facts: False
      roles:
        - example

    ansible-playbook distribute_public_key.yml

Related links:
--------------

* <http://ansibleworks.com/>
* <http://docs.ansible.com/>
* <https://github.com/ansible/ansible-examples/>
