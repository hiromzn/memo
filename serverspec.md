# install

- basic install

```
gem install serverspec
```

### ansible_spec

- https://github.com/volanja/ansible_spec

- install

```
gem install ansible_spec
```

- use

```
ansiblespec-init
```

- .ansiblespec [Optional] 

  - By default, site.yml will be used as the playbook and hosts as the inventory file. 
  - You can either follow these conventions or you can customize the playbook and inventory using an .ansiblespec file.

  - sample:

```
---
-
  playbook: site.yml
  inventory: hosts
  vars_dirs_path: inventories/staging
  hash_behaviour: merge
```

- Environment variables [Optional] 

  - You can use environment variables with the rake command. They are listed below.

    - PLAYBOOK -- playbook name (e.g. site.yml)
    - INVENTORY -- inventory file name (e.g. hosts)
    - VARS_DIRS_PATH -- directory path containing the group_vars and host_vars directories (e.g. inventories/staging)
    - HASH_BEHAVIOUR -- hash behaviour when duplicate hash variables (e.g. merge)
    - SSH_CONFIG_FILE -- ssh configuration file path (e.g. ssh_config)
    - SSH_CONFIG_FILE take precedence over the path at ssh_args( -F "filename") in [ssh_connection] section of ansible.cfg

  - Environment variables take precedence over the .ansiblespec file.

- Variables

  - Ansible variables supported by following condition.

    - Playbook's variables are supported. If same variable is defined in different places, priority follows Ansible order.
    - Variables defined main.yml in role's tasks are not supported.
    - Inventory variables are not supported.
    - Facts are not supported.

