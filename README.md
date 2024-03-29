# DEVOPS notes 2024
## SSH key setup

Generate SSH public and private keys

```
ssh-keygen -t ed25519 -C "key name"
```

Add SSH key to server

```
ssh-copy-id -i ~/.ssh/name_of_key.pub username@172.16.250.133
```

## Ansible

_Create inventory file. This composed of the list of hosts._

Connect to each hosts

```
ansible all --key-file ~/.ssh/private_key -i inventory -m ping
```

Creating Ansible Config

_filename_: ansible.cfg

```
[defaults]
inventory = inventory_directory
private_key_file = ~/.ssh/private_key
```

Ansible shortcode
```
ansible all -m ping
```

Ansible list hosts
```
ansible all --list-hosts
```

Ansible gather information

```
ansible all -m gather_facts --limit usernam@172.16.247.212
```


### Ansible elevated ad-hoc commands

apt update
```
ansible all -m apt -a update_cache=true --become --ask-become-pass
```

install app
```
ansible all -m apt -a name=package_name --become --ask-become-pass
```

upgrade app
```
ansible all -m apt -a "name=package_name state=latest" --become --ask-become-pass
```

sudo apt dist-upgrade
```
ansible all -m apt -a "upgrade=dist" --become --ask-become-pass
```

## Ansible Playbook

sample install_package.yml

```
---

- hosts: all
  become: true
  tasks:
  
  - name: update repository index
    apt:
      update_cache: yes

  - name: install java runtime package
    apt:
      name: default-jre
      state: latest

  - name: remove package
    apt:
      name: package_name
      state: absent

  - name: install dependencies via apt
    apt:
      name:
        - ca-certificates
        - curl
        - gnupg
```

run playbook
```
ansible-playbook --ask-become-pass install_package.yml
```

