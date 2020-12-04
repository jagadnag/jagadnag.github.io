---
layout: post
title: Ansible Part 1 - Intro
---

#### Topics covered:
- Ansible Intro
- Ansible 2.10 installation
- Initial setup, configure ansible.cfg and hosts file
- Learn to use ansible command line module
- Learn to use ansible-doc and online documentation

### Ansible Intro
Ansible is an awesome network management tool. It depends on how much time you are spending with Ansible to master the basic concepts and apply these concepts on the day to day work. Ansible is a very powerful tool and it can be the swiss army knife in your networking toolset. As you mature as a network automation engineer, you will start using the tool more often than managing the network devices on an individual basics. 

I am not covering much of the basics here, there are lot of free webinars available in ansible website to get yourself upto speed.

Refer the below mentioned link for more info:

https://www.ansible.com/resources/webinars-training
https://docs.ansible.com/ansible/latest/index.html


Once you get familar with all the basic concepts of ansible, you can start working on the below mentioned tasks. This is more of a DIY style approach, rather than me teaching everything. 

### Ansible 2.10 Installation

### Objective
- Setup your ansible environment: ansible.cfg & hosts file
- Run Ansible ad-hoc commands
- Develop a basic playbook
- Use cisco-ios modules to collect information and configure devices

#### Lab Setup
- GNS3, ENV-NG running cisco virutal devices

#### Code Editor
- Microsoft VS Code or editor of your choice


Refer the link for python setup:

https://www.digitalocean.com/community/tutorial_series/how-to-install-and-set-up-a-local-programming-environment-for-python-3





### Installation and setup

*It is highly recommeded to install and setup ansible inside a python virutal environment*

These are the required steps:
1) Create a project folder
2) Create a new python virtual enviorment 
3) Activate venv and install ansible using pip
4) Install paramiko SSH module via pip


```
jagadnag@ ~/projects$ mkdir ansible_2.10
jagadnag@ ~/projects$ cd ansible_2.10/
jagadnag@ ~/projects/ansible_2.10$ python3 -m venv venv
jagadnag@ ~/projects/ansible_2.10$ source venv/bin/activate
(venv) jagadnag@ ~/projects/ansible_2.10$
(venv) jagadnag@ ~/projects/ansible_2.10$
(venv) jagadnag@ ~/projects/ansible_2.10$ pip install ansible
Collecting ansible
  Downloading ansible-2.10.4.tar.gz (28.6 MB)
     |████████████████████████████████| 28.6 MB 5.7 MB/s
Collecting ansible-base<2.11,>=2.10.3
  Downloading ansible-base-2.10.3.tar.gz (5.8 MB)
     |████████████████████████████████| 5.8 MB 3.2 MB/s
Collecting jinja2
  Using cached Jinja2-2.11.2-py2.py3-none-any.whl (125 kB)
Collecting PyYAML
  Using cached PyYAML-5.3.1.tar.gz (269 kB)
Collecting cryptography
  Using cached cryptography-3.2.1-cp35-abi3-macosx_10_10_x86_64.whl (1.8 MB)
Collecting packaging
  Downloading packaging-20.7-py2.py3-none-any.whl (35 kB)
Collecting MarkupSafe>=0.23
  Using cached MarkupSafe-1.1.1.tar.gz (19 kB)
Collecting six>=1.4.1
  Using cached six-1.15.0-py2.py3-none-any.whl (10 kB)
Collecting cffi!=1.11.3,>=1.8
  Using cached cffi-1.14.4-cp39-cp39-macosx_10_9_x86_64.whl (177 kB)
Collecting pyparsing>=2.0.2
  Using cached pyparsing-2.4.7-py2.py3-none-any.whl (67 kB)
Collecting pycparser
  Using cached pycparser-2.20-py2.py3-none-any.whl (112 kB)
Using legacy 'setup.py install' for ansible, since package 'wheel' is not installed.
Using legacy 'setup.py install' for ansible-base, since package 'wheel' is not installed.
Using legacy 'setup.py install' for PyYAML, since package 'wheel' is not installed.
Using legacy 'setup.py install' for MarkupSafe, since package 'wheel' is not installed.
Installing collected packages: MarkupSafe, jinja2, PyYAML, six, pycparser, cffi, cryptography, pyparsing, packaging, ansible-base, ansible
    Running setup.py install for MarkupSafe ... done
    Running setup.py install for PyYAML ... done
    Running setup.py install for ansible-base ... done
    Running setup.py install for ansible ... done
Successfully installed MarkupSafe-1.1.1 PyYAML-5.3.1 ansible-2.10.4 ansible-base-2.10.3 cffi-1.14.4 cryptography-3.2.1 jinja2-2.11.2 packaging-20.7 pycparser-2.20 pyparsing-2.4.7 six-1.15.0
WARNING: You are using pip version 20.2.3; however, version 20.3.1 is available.
You should consider upgrading via the '/Users/jagadnag/projects/ansible_2.10/venv/bin/python3 -m pip install --upgrade pip' command.
(venv) jagadnag@ ~/projects/ansible_2.10$
(venv) jagadnag@ ~/projects/ansible_2.10$ pip install paramiko
Collecting paramiko
  Using cached paramiko-2.7.2-py2.py3-none-any.whl (206 kB)
Collecting bcrypt>=3.1.3
  Using cached bcrypt-3.2.0-cp36-abi3-macosx_10_9_x86_64.whl (31 kB)
Collecting pynacl>=1.0.1
  Using cached PyNaCl-1.4.0-cp35-abi3-macosx_10_10_x86_64.whl (380 kB)
Requirement already satisfied: cryptography>=2.5 in ./venv/lib/python3.9/site-packages (from paramiko) (3.2.1)
Requirement already satisfied: cffi>=1.1 in ./venv/lib/python3.9/site-packages (from bcrypt>=3.1.3->paramiko) (1.14.4)
Requirement already satisfied: six>=1.4.1 in ./venv/lib/python3.9/site-packages (from bcrypt>=3.1.3->paramiko) (1.15.0)
Requirement already satisfied: pycparser in ./venv/lib/python3.9/site-packages (from cffi>=1.1->bcrypt>=3.1.3->paramiko) (2.20)
Installing collected packages: bcrypt, pynacl, paramiko
Successfully installed bcrypt-3.2.0 paramiko-2.7.2 pynacl-1.4.0
WARNING: You are using pip version 20.2.3; however, version 20.3.1 is available.
You should consider upgrading via the '/Users/jagadnag/projects/ansible_2.10/venv/bin/python3 -m pip install --upgrade pip' command.
(venv) jagadnag@ ~/projects/ansible_2.10$
```

- Optionally export the packages installed in your virtual environment to a `requirements.txt` file. This file could be shared with others so that they are able to reproduce your virtual environment.

    `pip list > requirements.txt`

- You can exit your current virtualenv using the "deactivate" command.

### Verify Ansible Insallation

Run the command "ansible --version" to verify the ansible version

```
(venv) jagadnag@ ~/projects/ansible_2.10$ ansible --version
ansible 2.10.3
  config file = None
  configured module search path = ['/Users/jagadnag/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /Users/jagadnag/projects/ansible_2.10/venv/lib/python3.9/site-packages/ansible
  executable location = /Users/jagadnag/projects/ansible_2.10/venv/bin/ansible
  python version = 3.9.0 (default, Oct 27 2020, 14:13:35) [Clang 11.0.0 (clang-1100.0.33.17)]
(venv) jagadnag@ ~/projects/ansible_2.10$
```

Ping the localhost to confirm ansible is working properly.

```
(venv) jagadnag@ ~/projects/ansible_2.10$ ansible -m ping localhost
[WARNING]: No inventory was parsed, only implicit localhost is available
localhost | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
(venv) jagadnag@ ~/projects/ansible_2.10$
```

### Ansible Modules and Documenation

Ansible by default ships with several modules, you can access the module documenation using two ways:

#### Run Ansible doc command

    `ansible-doc -l` > to list all the modules. Exit by pressing 'q'

    `ansible-doc cisco.ios.ios_command` > type the module name directly and read through the documentation.

- Ansible Online doumentation:
    https://docs.ansible.com/ansible/latest/collections/index.html


## 1.4 Configure the ansible.cfg file

All ansible related configuration can be configured the ansible.cfg file. As a best practise you will be creating the file in the project directory.

#### Create a new file named ansible.cfg and apply the below mentioned config

More info - https://docs.ansible.com/ansible/2.8/reference_appendices/config.html#ansible-configuration-settings

```
[defaults]
gathering = explicit
inventory = hosts.yml

# SSH Host keys
host_key_checking = False
host_key_auto_add = True

# Settings to remove warnings
deprecation_warnings = False
retry_files_enabled = False
interpreter_python = auto_silent
```

## 1.5 Create Ansible Inventory

All details related to the managed hosts are provided through inventory file. Ansible inventory can be created manually or dynamically using ini or yaml syntax. For more details refer to - https://docs.ansible.com/ansible/2.8/user_guide/intro_inventory.html

#### Create a new file named hosts.yml and define your managed hosts (csr1000v) details. 

```
[cisco]
csr01 ansible_host=192.168.x.x (change the ip address)

[cisco:vars]
ansible_user=cisco
ansible_password=cisco
ansible_connection=network_cli
ansible_network_os=ios
```

#### Task:
- Create two groups 'core' and 'branch', define few dummy hosts under it.
- Create a parent group named 'routers' and add core and branch to it.
- Define the ansible_user and ansible_password variables for the routers group.

## 1.6 Ansible Command line

Ansible command line can be very useful to run quick tasks, which dosent need a playbook. You are required to pass in all the required details as arguments. For ex: hosts file or host details, module, module parameters, connection paramertes etc.,

#### Run the below mentioned commands and check the results

  `ansible --help`

  `ansible --list-hosts all`

  `ansible <host or grp name> -m ios_facts`

  `ansible <host or grp name> -m ios_command -a "commands='show version'" `

  `ansible <host or grp name> -m ios_command -a "commands='show ip int brie'" > sip.txt`

## 1.7 Develop a basic playbook

#### Create a basic playbook and run it using the `ansible-playbook` command. 

- Example playbook:

```
---
- name: Ping CSR01
  hosts: cisco

  tasks:
    - name: lauching Ping
      ping:
```

## 1.8 Write playbook using cisco-ios modules

Refer cisco ios module documentaion and write simple playbooks.

#### Task 1:
- Write a playbook to collect show version command output
- Use the debug module to print the output to the screen

#### Task 2:
- Write a playbook to configure a new syslog-server
- Use the debug module to print the output to the screen

#### Task 3:
- Write a playbook to use ios_facts module
- Use the debug module to print the facts to the screen

Example playbook, pls refer to module documenation and call the correct modules and arguments. 

```
---
- name: Collect show version
  hosts: cisco

  tasks:
    - name: run show version on remote devices
      ios_command:
        commands: show version
      register: output

    - debug: var=output.stdout_lines
```