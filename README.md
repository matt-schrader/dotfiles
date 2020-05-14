# Sway Acid Dark 

[![Build Status](https://travis-ci.org/eoli3n/dotfiles.svg?branch=master)](https://travis-ci.org/eoli3n/dotfiles)

**Sway fish pure waybar neovim**

![alt tag](https://github.com/eoli3n/dotfiles/blob/master/screenshots/sway.png)

**Tiny irc client**

![alt tag](https://github.com/eoli3n/dotfiles/blob/master/screenshots/irc.png)

**Firefox/Tabliss Wofi**

![alt tag](https://github.com/eoli3n/dotfiles/blob/master/screenshots/ff.png)

**Connman/Thunar GTK Theme**

![alt tag](https://github.com/eoli3n/dotfiles/blob/master/screenshots/gtk.png)

### Why Ansible ?

- Modularity: [Roles](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html) as modules.
- Factorization: It uses [jinja2](https://docs.ansible.com/ansible-container/container_yml/template.html) templating engine.
- Simplicity: No agent, only SSH, available via pip.
- Flexibility: Push your dotfiles from/to any hosts.
- Toolbox: Dry-run mode, diff mode, secrets encryption, tags...

### How to

**Use carefully**, backup your home before using !  
You should use ``--check`` and ``--diff`` to dryrun first.

Check details in ``roles/*/README.md``.  

##### 1. Fork Me!

##### 2. Clone your repo

```
git clone https://github.com/*/dotfiles
```

Then, configure desktop environment in [group_vars/all.yml](group_vars/all.yml).

##### 3. Configure inventory

Create inventory file from template.

```
cd dotfiles
cp hosts.template hosts
```

Add your hostnames in section:
- ***cli***: install only cli tools
- ***desktop***: install cli tools + desktop environment  

Define which user will get configurations with *ansible_user* var.  
- *desktop* hosts **can't use root**.  

###### a. localhost run

Let's use a trick to let ansible think that there is 2 different hosts.  
It will configure *root* with *cli* tools only and *user* with *desktop* environment.
That trick needs ``-K`` without ``-b`` when running playbook.

```
[cli]
cli_user ansible_connection=local ansible_user=root
[desktop]
desktop ansible_connection=local ansible_user=user
```

###### b. multiple hosts run

```
[cli]
server1 ansible_user=root
[desktop]
host1 ansible_user=user
host2 ansible_user=user2
```

##### 4. Configure SSH

Push your SSH public key on all your ``users@hosts``
```
ssh-copy-id -i path/to/ssh/key.pub user@host
```

##### 5. (Dry)Run

```
ansible-playbook install.yml -CD
ansible-playbook install.yml
```
To configure cli tools for root on desktop hosts:  
*Note: ansible_user needs to be sudoers.*  
```
ansible-playbook install.yml -b -K -l desktop
```

-----

##### Previously

* [i3-gaps Dark Solarized](https://github.com/eoli3n/dotfiles/tree/zsh-agnoster-solarized)
* [i3-gaps Acid Dark](https://github.com/eoli3n/dotfiles/tree/i3-gaps-acid-dark)
