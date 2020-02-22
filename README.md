Ansible-role-backup-ale
=========

ALE OmniSwitch config backup via ansible playbook

Support
------------
* Standalone
* Stack
* Virtual Chassis

Requirements
------------

1. Login to OmniSwitch via Public Key Authentication (PKA)

|Device Version|Key Type|Key Path|Key Filename|
|:-:|:-:|:-:|:-:|
|R6|DSA|/flash/network/pub/|`{device_user}`_dsa.pub|
|R7~|RSA or DSA|/flash/system/|`{device_user}`_`{rsa,dsa}`.pub|

AOS R7~8 need to use `installsshkey` command for the specified user
```
> installsshkey new_ssh_user /flash/system/new_ssh_user_rsa.pub
```

2. Predefined ncftp bookmark


Role Variables
--------------

### defaults

|variable name|default|description|
|:-:|:-:|:-:|
|device_user|admin|AOS user account|
|device_port|22|AOS ssh port|

### vars

|variable name|default|description|
|:-:|:-:|:-:|
|ssh_option|"StrictHostKeyChecking=no"|bypass ssh host key|
|backup_local_path|"{{ role_path }}/files"|temp backup file path|
|backup_srv_path|"/backup/network/{{ inventory_hostname }}/"|backup server file path|
|backup_vc_filename|"{{ lookup('pipe', 'date +%Y%m%d') }}_{{ inventory_hostname }}_vcboot.cfg"|virtual chassis backup filename|
|backup_std_filename|"{{ lookup('pipe', 'date +%Y%m%d') }}_{{ inventory_hostname }}_boot.cfg"|backup filename|


Example Inventory
----------------

Example of inventory file

```
AOSR6 ansible_host=192.168.1.1 ansible_os_family=std
AOSR8 ansible_host=192.168.1.2 ansible_os_family=vc

[ale]
AOSR6
AOSR8
```

Example Playbook
----------------


```yaml
- hosts: ale
  gather_facts: no
  connection: local
  tags: ale

  roles:
    - backup-ale
```


Author Information
------------------

Sam Chen
