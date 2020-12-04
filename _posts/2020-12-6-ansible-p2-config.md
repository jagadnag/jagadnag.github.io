---
layout: post
title: Ansible Part 2 - Basic setup
---

#### Topics covered:
- Ansible Building blocks
- Initial setup, ansible.cfg configuration
- Host Inventory
- Learn to use ansible command line module


### Ansible 2.10 changes

Ansible team brought major changes to the ansible 2.10 version. There are two key components of ansible that we need to remember **ansbile-base** and **ansible-collections**

The ansible/ansible (ansible-base) repository only contains:

#### Ansible Base:

Base package contains the core Ansible programs and documentation. The ansible package (will contain a subset of collections) and depend on the new ansible-base package (the Ansible engine).A tiny subset of modules and plugins are also included with the base package. For ex: cisco ios, iosxr, nxos etc.,

The rest of the modules and plugins have been moved into various "collections"

#### Ansible Collections:

Collections are independently developed and published by the person or team who is developing and maintaning the modules. The collections will be hosted on ansible-galaxy, where we can install it on demand basis.

The bigger picture is more like how to work with our phones. The base operation system (Andriod or iOS) is like the ansible-base, it comes with few native apps to get started. The ansible-collections are like the apps that we download from the app store, not everyone will need all the apps, so we can pick and choose based on our requirement to complete the task in hand. 


## Configure the ansible.cfg file

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

## Create Ansible Inventory

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

## Ansible Command line

Ansible command line can be very useful to run quick tasks, which dosent need a playbook. You are required to pass in all the required details as arguments. For ex: hosts file or host details, module, module parameters, connection paramertes etc.,

#### Run the below mentioned commands and check the results

  `ansible --help`

  `ansible --list-hosts all`

  `ansible <host or grp name> -m ios_facts`

  `ansible <host or grp name> -m ios_command -a "commands='show version'" `

  `ansible <host or grp name> -m ios_command -a "commands='show ip int brie'" > sip.txt`

## Develop a basic playbook

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

## Write playbook using cisco-ios modules

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