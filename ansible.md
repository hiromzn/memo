# manual

- online manual

  ```
  $ ansible-doc file
  $ ansible-doc -h
  ```

# directory structure

```
./site.yml
	- hosts: webservers
	  become: yes
	  roles:
	    - <role1>
	    - <role2>

./<inentory_file>

./roles/	# <inventory_file�Ɠ��K�w

./roles/common/vars/main.yml	# role�ɋ��ʂ����ϐ��̒�`

./roles/<role_name>/
./roles/<role_name>/tasks/
./roles/<role_name>/tasks/main.yml
	- include <task1.yml>
./roles/<role_name>/tasks/*.yml
	- name: task1
	  yum: name=httpd state=latest
	  notify: handler����
./roles/<role_name>/handlers/
./roles/<role_name>/handlers/main.yml
	- include <handler1.yml>
./roles/<role_name>/handlers/*.yml
	- name: handler����
	  service: name=httpd state=restarted
./roles/<role_name>/files/
./roles/<role_name>/templates/
./roles/<role_name>/vars/main.yml
./roles/<role_name>/defaults/main.yml
./roles/<role_name>/meta/

./group_vars/<group_name>/01_core
./group_vars/<group_name>/02_os
./group_vars/<group_name>/03_db
./group_vars/all/***
./group_vars/ungrouped/***
./host_vars/<host_name>

```

### playbook

- playbook = play�̃��X�g

- play
  - host�̏W�� & task�̃��X�g

  - name : play�̓��e
  - become : sudo�Ƃ��Ă̓���
  - vars : �ϐ�

- task

  - name: .....
  - <module_name>: <args> ...
  
    EX) apt: name=enginx update_cache=yes

- module

  - apt : package manager
  - copy : file copy
  - file : various file
  - service :
  - template: creating file from template

### vars, �ϐ�

##### vars�i�ϐ��j�T�v

- vars�Œ�`�����ϐ��̃X�R�[�v�́A���s���̃z�X�g����уz�X�g��������O���[�v

  - ����I�ɂ́A����O���[�v���ɑ�����e�z�X�g�ɑ΂��Đ��������B
  - hostvars.<hostname>.XXXX�Ƃ����ϐ����쐬�����킯�ł͂Ȃ��A���s���ɑ΂��č쐬�����Ǝv����B

- �ϐ���`�̗D�揇��

  - �������O�̕����̕ϐ����قȂ�ꏊ�Œ�`����Ă���ꍇ����̏����ŏ㏑�������B
  - ��܂��ȗD�揇�ʁF�i�D�揇���������̂��烊�X�g�j
    - commandline -e(--extra-vars)�F�ō�����
    - ���̃��X�g�ȊO
    - host_vars / group_vars
    - fact
    - role/defaults/main.yml�F�Œ�
   
  - URL: https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#variable-precedence-where-should-i-put-a-variable

  - �D�揇�ʁi�D�揇�ʂ��ŒႩ��ō��̏��ɕ���ł���j
    - command line values (eg �g-u user�h)
    - role defaults [1]
    - inventory file or script group vars [2]
    - inventory group_vars/all [3]
    - playbook group_vars/all [3]
    - inventory group_vars/* [3]
    - playbook group_vars/* [3]
    - inventory file or script host vars [2]
    - inventory host_vars/* [3]
    - playbook host_vars/* [3]
    - host facts / cached set_facts [4]
    - play vars
    - play vars_prompt
    - play vars_files
    - role vars (defined in role/vars/main.yml)
    - block vars (only for tasks in block)
    - task vars (only for the task)
    - include_vars
    - set_facts / registered vars
    - role (and include_role) params
    - include params
    - extra vars (always win precedence)
      - ansible-playbook -e NAME=value : --extra-vars="NAME=value"

- �ϐ��̗D�揇�ʁF�ϐ��͂ǂ��ɒu���ׂ����H

  - �ϐ������̕ϐ����ǂ̂悤�ɏ㏑�����邩�ɂ��čl������A�����ǂ��ɒu���ׂ�����m���Ă���ق����d�v

  - ��{�I��var�D�惋�[��
    - �u���[���f�t�H���g�v�i���[�����̃f�t�H���g�t�H���_�j�ɓ�����̂͏㏑�������B
    - �z�X�g�ϐ���C���x���g���ϐ��̓��[���̃f�t�H���g�ɏ���B
    - ���[����vars�f�B���N�g���ɂ�����̂͂��ׂāA���O��ԓ��̂��̕ϐ��̈ȑO�̃o�[�W�������I�[�o�[���C�h����B
    - �R�}���h���C�����D�悳���B

  - var��`���[��
    - �ǂ̃Z�N�V�������ł��Avar���Ē�`����ƑO�̃C���X�^���X���㏑������܂��B
    - hash_behaviour
      - replace
      - merge
    - �O���[�v���[�f�B���O
      - �e�q�֌W�ɏ]���܂��B
      - �����u�e/�q�v���x���̃O���[�v�́A�A���t�@�x�b�g���Ƀ}�[�W����܂��B
      	- ansible_group_priority�ŏ㏑���\
	- �C���x���g���\�[�X�ł̂ݐݒ�ł��Agroup_vars /�ł͐ݒ�ł��܂���B

    - inventory�ł�ansible_ssh_user�ݒ�́A�R�}���h���C�����܂ޑ��̑S�Ă��D�悳���B
      - inventory: ansible_ssh_user > commandline: -u <user>
      - inventory: ansible_ssh_user > play/task: remote_user: <user>

    - remote_user��inventory���܂߂ď㏑���������ꍇ�́A�ǉ��̕ϐ����g�p����B
    
      - commandline: -e "ansible_user=<user>" > inventory: ansible_ssh_user

  - �ϐ��̃X�R�[�v

    - �O���[�o��
      - �ݒ�ꏊ
      	- config
	- ���ϐ�
	- �R�}���h���C��
    - Play
      - ���ꂼ���play�Ƃ���Ɋ܂܂��\����
      - �ݒ�ꏊ�F
      	- vars�G���g���ivars; vars_files; vars_prompt�j
	- ���[���f�t�H���g
	- vars
    - �z�X�g
      - �z�X�g�ɒ��ڊ֘A�t�����Ă���ϐ�
      - �ݒ�ꏊ
      	- �C���x���g��
	- include_vars
	- �t�@�N�g
	- registered : �o�^�ς݃^�X�N�o��

### �ė��p�\��Playbook : role / include / import

- https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse.html

- include / import
  - ������Playbook�������́A����Playbook���ł̕����񗘗p�Ŏg���B

  - ���ׂĂ�import*�X�e�[�g�����g�́A�v���C�u�b�N�̉�͎��ɑO��������܂��B
  - ���ׂĂ�include*�X�e�[�g�����g�́A�v���C�u�b�N�̎��s���ɔ��������Ƃ��ɏ�������܂��B


- include_task / import_task
  - �C�ӂ̐[���Ŏg�p�\
  - �ϐ���n�����Ƃ��\
    ```
    - import/include_task: my_tasks.yml
      vars:
        my_user: foo
    - import/include_task: my_tasks.yml
      vars:
        my_user: bar
    ```
  - �ÓI�Ɠ��I�����݂����邱�Ƃ͂ł��܂����A�v���C�u�b�N�̐f�f������o�O�ɂȂ���\�������邽�߁A�����߂ł��܂���B
  - import�����include�ɕϐ���n�����߂�key = value�\���͐�������Ă��܂���B�����YAML vars�F���g�p���Ă��������B
  - �C���N���[�h�ƃC���|�[�g��handler�F�Z�N�V�����ł��g�p�ł��܂��B

- role
  - �^�X�N�ȏ�̂��̂��ꏏ�Ƀp�b�P�[�W�����邱�Ƃ��\
  - �ϐ��A�n���h���A���邢�̓��W���[���⑼�̃v���O�C���������܂ނ��Ƃ��ł���B
  - include / import �Ƃ͈قȂ�A���[����Ansible Galaxy����ăA�b�v���[�h����ы��L���邱�Ƃ��\�B

- dynamic / static
  - Ansible 2.0����Adynamic include ���������ꂽ�B
  - Ansible 2.1����Ainclude �������I�ɐÓI�ɂ���@�\���������ꂽ�B
  - Ansible 2.4����Ainclude �� import���������ꂽ�B

  - import*�iimport_playbook�Aimport_tasks�Ȃǁj���g�p����ƁA�ÓI�ɂȂ�B
  - include*�iinclude_tasks�Ainclude_role�Ȃǁj���g�p����ƁA���I�ɂȂ�B

  - �Ⴂ
    - Ansible�́APlaybook�̉�͒��ɂ��ׂĂ̐ÓI�C���|�[�g��O�������܂��B
    - dynamic include�́A���̃^�X�N�����o���ꂽ���_�Ŏ��s���ɏ�������܂��B

    - tags ������t���X�e�[�g�����g�iwhen :)�ȂǁAAnsible�^�X�N�I�v�V�����Ɋւ��ẮF

    - �ÓI�C���|�[�g�̏ꍇ�A�e�^�X�N�I�v�V�����̓C���|�[�g���Ɋ܂܂�邷�ׂĂ̎q�^�X�N�ɃR�s�[����܂��B
    - ���I�C���N���[�h�̏ꍇ�A�^�X�N�I�v�V�����͕]�����ɓ��I�^�X�N�ɂ̂ݓK�p����A�q�^�X�N�ɂ̓R�s�[����܂���B

    - ���� : role
      - Ansible 2.3���O
      	- ���[���͏�ɁArole: �I�v�V��������ĐÓI�Ɋ܂܂�邱�ƂŁA���̂ǂ̃v���C�^�X�N�����O�ɏ�ɍŏ��Ɏ��s����܂����B
      - Ansible 2.3����
      	- ���[���𑼂̃^�X�N�ƈꏏ�ɃC�����C���Ŏ��s�ł���悤�ɂ��邽�߂�include_role�I�v�V��������������܂����B

- include vs import

  - include*�X�e�[�g�����g
  
    - ��ȗ��_�̓��[�v
    - ���[�v���C���N���[�h�ƂƂ��Ɏg�p����ƁA�܂܂��^�X�N�܂��̓��[���̓��[�v���̊e���ڂɑ΂���1����s����܂��B

  - include*�̐���

    - ���I�C���N���[�h���ɂ̂ݑ��݂���^�O��--list-tags�̏o�͂ɕ\������܂���B
    - ���I�C���N���[�h���ɂ̂ݑ��݂���^�X�N��--list-tasks�̏o�͂ɕ\������܂���B
    - notify���g���āA���I�C���N���[�h�����痈��n���h�������N�����邱�Ƃ͂ł��܂���i���L�̒����Q�Ɓj�B
    - ���I�C���N���[�h���̃^�X�N������s���J�n���邽�߂�--start-at-task���g�p���邱�Ƃ͂ł��܂���B

  - import*�̐���
    
    - ��L�̂悤�ɁA���[�v�̓C���|�[�g�ł͂܂������g�p�ł��܂���B
    - �^�[�Q�b�g�t�@�C���܂��̓��[�����ɕϐ����g�p����ꍇ�A�C���x���g���\�[�X����̕ϐ��i�z�X�g/�O���[�v�ϐ��Ȃǁj�͎g�p�ł��܂���B

  - ���� : ���I�^�X�N�ւ�notify�̎g�p�Ɋւ���
  
    - ���I�C���N���[�h���̂��N�����邱�Ƃ͉\
    - ����ɂ��A�C���N���[�h���̂��ׂẴ^�X�N�����s����܂��B
    
##### defined vars

- hostvars
  - �S�Ẵz�X�g�ɑ΂��Ē�`���ꂽ�S�Ă̕ϐ����܂ގ���
  - �z�X�g�����L�[�ɂȂ��Ă���̂�fact�Ƃ��ĎQ�Ƃł���B
    ```
    {{ hostvars['db.exsample.com'].ansible_eth1.ipv4.address }}
    ```
�@- ���݂̃z�X�g�Ɋւ���S���(fact)�̕\��
    ```
    # playbook task
    - debug: var=hostvars[inventory_hostname]
    ```

- inventory_hostname
  - ansible���m���Ă��錻�݂̃z�X�g��

- group_names
  - ���݂̃z�X�g���O���[�v�ɂȂ��Ă���S�O���[�v��

- groups
  - �O���[�v�����L�[�ŁA�����o�[�̃z�X�g���l�Ƃ��鎫��
  - ex) {"all":[...], "web":[...], ...}

- play_hosts
  - ����play���ŃA�N�e�B�u�ɂȂ��Ă���C���x���g���̃z�X�g���̃��X�g

- ansible_version
  - ex) 

##### tasks

- set_fact
  - fact��ݒ�E�ǉ�����^�X�N�i�V�ϐ����`�j

```
  - set_fact: ansp={{ snap_result.stdout }}
```
##### how to use vars

- �ϐ��̒�`�Fplaybook���Œ�`

```
# format : 
#
# vars:
#   <var_name>: <value>

# sample:
  vars:
    key_file: /etc/...
    conf_file: /etc/xx.conf
    server_name: node01
```

- �ϐ��̒�`�F�t�@�C���ł̒�`

```
# playbook�̋L�q
  vars_files:
    - myvars.yml

# myvars.yml
key_file: /etc/...
conf_file: /etc/xx.conf
server_name: node01
```

- �ϐ��̒l�̕\��

```
# print var
- debug: var=<var_name>
```

- �^�X�N���s���ʂ̕ϐ��ւ̓o�^(register)

```
- name: capture output of whoami command
  command: whoami
  register: myoutput

- debug: var=myoutput
- debug: msg="stdout of whoami : {{myoutput.stdout}}"
- debug: msg="stderr of whoami : {{myoutput.stderr}}"
- debug: msg="return value of whoami : {{myoutput.rc}}"
- debug: msg="start time of whoami : {{myoutput.start}}"
# attribute of myoutput
#   changed
#   cmd
#   start, end, delta # time ( delta = end - start )
#   stdout
#   stderr
#   stdout_lines
#   rc # return value
```

#### format

### YAML

#### format

```
---
# comment
```

- stiring
  this is string
  or
  "this is string"

- True False

- list ( sequence )
```
- list1
- list2

JSON:
[
  "list1",
  "list2"
]
```

- dictionary (object:JSON)

```
address: 1-2-3 aaa streat
city: ddfdaf city
state: North Takoma

JSON:
{
  "address": "....",
  "cyty": "....",
  "state": "N..."
}
```

- append / merge to list or dictionaries

  - https://serverfault.com/questions/685673/appending-to-lists-or-adding-keys-to-dictionaries-in-ansible
  - https://stackoverflow.com/questions/31772732/ansible-how-to-keep-appending-new-keys-to-a-dictionary-when-using-set-fact-mod
  
```
  vars:
    mylist:
      - ext1
      - ext2
      - ext3
    append_exts:
      - ext4
      - ext5
    dict: [ { uid: 111, gid: 222 } ]
    add_dict: [ { uid: 1234, gid: 333 } ]

  tasks:
    - name: '{{ append_exts }}'
      debug: var=append_exts

    - name: '{{ mylist }}'
      debug: var=mylist

    - name: append
      set_fact:
        mylist: "{{ mylist + append_exts }}"

    - name: '{{ mylist }}'
      debug: var=mylist

    - name: "append dict {{ dict }}"
      set_fact:
        dict: '{{ dict + add_dict }}'

    - name: 'new dict: {{ dict }}'
      debug: var=dict
```

- �s�̐܂�Ԃ��i�����s�ł̕\�L�j

```
address: >
  address1
  address2
  address3
  etc...

JSON:
{
  "address": "address1
  	      address2
	      ...",
  "next": ...
}
```

## inventory

- ref : https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html

- ��`�`��
  - INI-like
  - YAML

- default group
  - �Q�̃f�t�H���g�O���[�v������B
    - all
      - �S�Ẵz�X�g��������O���[�v
    - ungrouped
      - ����̃O���[�v�ɑ����Ă��Ȃ��z�X�g��������O���[�v
  - �S�Ẵz�X�g�́A���Ȃ��Ƃ��Q�̃O���[�v�̑�����B
    - all and ungrouped
    - all and ����̃O���[�v�i�P�ȏ�j

- group
  - childlen group
    - �q�O���[�v�̃����o�[�ł���z�X�g�́A�����I�ɐe�O���[�v�̃����o�[�ɂȂ�B
    - �q�O���[�v�̕ϐ��́A�e�O���[�v�̕ϐ������D�悳���i�㏑������܂��j�B
    - �O���[�v�͕����̐e�Ǝq�������Ƃ��ł��܂����A�z�֌W�͂ł��Ȃ��B
    - �z�X�g�͕����̃O���[�v�ɑ����邱�Ƃ��ł��邪�A�z�X�g�̃C���X�^���X��1�����Ȃ��A�����̃O���[�v����̃f�[�^���}�[�W�����B

- �z�X�g�ƃO���[�v�ŗL�̃f�[�^�̕���
  - Ansible�ł́A���C���C���x���g���t�@�C���ɕϐ����i�[���Ȃ����Ƃ������߂��܂��B
  - �z�X�g�ϐ��ƃO���[�v�ϐ��͕ʁX�̃t�@�C���Ɋi�[�\�B
  - �ϐ��t�@�C����YAML�`���i�g���q�F.yml, .yaml, .json, �܂��͊g���q�Ȃ��j

- group_vars/ and host_vars/

  - �i�[�f�B���N�g���F�@group_vars/ and host_vars/
    - playbook�f�B���N�g���܂���inventory�f�B���N�g���ɔz�u�\�B
    - �����̃p�X�����݂���ꍇ�Aplaybook�f�B���N�g���̕ϐ��́Ainventory�f�B���N�g���ɐݒ肳��Ă���ϐ����㏑�����܂��B

  - �Ǘ����@
    - �C���x���g���t�@�C���ƕϐ���Git���|�W�g���ɕۑ����邱�Ƃ�����
    - �C���x���g���ƃz�X�g�ϐ��̕ύX��ǐՉ\�ɂȂ邽�߁B

- file and directory structure

  - one file

```
# ./<inventory_file>

<hostname1>
<hostname2>

[<group_name1>]
<hostname3>	<var1>=111 <var2>=222	# �z�X�g�ϐ�
<hostname4>	<var1>=111 <var2>=222	# �z�X�g�ϐ�

[<group_name3>:children]
<group_name1>
<group_name2>

[<group_name3>:vars]
<var1>=111	# �O���[�v�ϐ�
<var2>=222	# �O���[�v�ϐ�

[all:vars]
<var1>=111
<var2>=222
```


  - BASIC

```
#
# [BASIC] host / group vars directory structure
#
./<inventory_file>
./group_vars/prodction_group	# production_group�ϐ��̒�`�t�@�C��
./group_vars/develop_group
./group_vars/all
./group_vars/ungrouped
./host_vars/host1		# host1�ϐ��̒�`�t�@�C��
./host_vars/host2

#
# file format of group/host_vars (only : *.yml or *.yaml or *.json)
#
<var1>: <value>
<var2>: <value>
```

  - ADVANCE

```
#
# [ADVANCE] host / group vars directory structure
#
# ==> group�ϐ��𕡐��̃t�@�C���ɕ�������ꍇ�B
#     �t�@�C���͎������ɓǂݍ��܂��B
#
./<inventory_file>
./group_vars/prodction_group/01_core	# production_group�ϐ��𕪊�
./group_vars/prodction_group/02_os
./group_vars/prodction_group/03_db
./group_vars/develop_group/01_core
./group_vars/develop_group/02_os
./group_vars/develop_group/03_db
./group_vars/all/***
./group_vars/ungrouped/***
./host_vars/host1
./host_vars/host2

```
- �ϐ��̃}�[�W
  - �f�t�H���g
    - �v���C�����s�����O�ɕϐ��͓���̃z�X�g�Ƀ}�[�W/�t���b�g�������
    - Ansible�̓z�X�g�ƃ^�X�N�ɏW�����Ă��邽�߁A�O���[�v�̓C���x���g���ƃz�X�g�̃}�b�`���O�ȊO�ɂ͗��p����Ȃ��B
    - Ansible�̓O���[�v��z�X�g�ɒ�`����Ă�����̂��܂߂ĕϐ����㏑������B
  - �f�t�H���g��ύX����ɂ́Ahash_merge �ݒ���Q��
  - �ϐ��̗D�揇�ʁi�ŒႩ��ō��A�ŏ��̂��̂͌�ɂ��̂ɏ㏑�������B�j

    - all (���ׂẴO���[�v)
    - �e�O���[�v
    - �q�O���[�v
    - �z�X�g

  - �����e�q���x���̃O���[�v���}�[�W�����ƁA�A���t�@�x�b�g���ɍs���A�Ō�Ƀ��[�h���ꂽ�O���[�v���O�̃O���[�v���㏑�����܂��B 

  - Ansible�o�[�W����2.4�ȍ~
  - �O���[�v�ϐ�ansible_group_priority���g�p���āA�������x���̃O���[�v�̃}�[�W����ύX�\�i�e�q�������������ꂽ��j
  - �������傫���قǁA��Ń}�[�W����A�D�揇�ʂ������Ȃ�܂��B 
  - �ݒ肳��Ă��Ȃ��ꍇ�A���̕ϐ��̃f�t�H���g��1

  - �ȉ��̗�ł́Aa_group�ɍ����D�揇�ʂ�^���邽�߁A���ʂ�testvar == a�ɂȂ�B
  
    ```
    a_group�F
	testvar�Fa
    	ansible_group_priority�F10
    b_group�F
	testvar�Fb
    ```
- inventory parameters

  - Host connection:
    - ansible_connection

  - General for all connections:

    - ansible_host
    - ansible_port
    - ansible_user

  - Specific to the SSH connection:

    - ansible_ssh_pass
      - �ݒ肷��ꍇ�́Aplain text�͎g�킸vault�Őݒ肷��B
    - ansible_ssh_private_key_file
    - ansible_ssh_common_args
    - ansible_sftp_extra_args
    - ansible_scp_extra_args
    - ansible_ssh_extra_args
    - ansible_ssh_pipelining
    - ansible_ssh_executable (added in version 2.2)

  - Privilege escalation

    - ansible_become
    - ansible_become_method
    - ansible_become_user
    - ansible_become_pass
    - ansible_become_exe
    - ansible_become_flags

  - Remote host environment parameters:

    - ansible_shell_type
    - ansible_python_interpreter
    - ansible_*_interpreter
    - ansible_shell_executable

  - ansible_connection=<..>

    - local
    - docker

```
# basic inventory
node1
node2
node3
mytest1 ansible_ssh_host=node1 ansible_ssh_port=22
mytest2 ansible_ssh_host=node1 ansible_ssh_port=22
#
# COMMENT : mytest1��mytest2�͈قȂ�z�X�g�Ƃ��Ĉ�����B
#
```

```
# group
[web]
node1

[app]
node2

[db]
node3
```

```
# nested group : ���d��`���\
[system_A:children]
web
app
db

[web]
node1

[app]
node2

[db]
node3
```

```
# nested group : ���d��`���\
[foo:children]
bar

[bar:children]
web
app

[system_A:children]
web
app
db

[web]
node1

[app]
node2

[db]
node3
```

##### host_vars / group_vars

- host�����group�ɑ΂���ϐ��̒�`���܂Ƃ߂Ď��{������@

- inventory file

```
# host vars
myhost1 ansible_ssh_host=1.2.3.4 ansible_ssh_port=22

# gorup vars
[all:vars]
ntp_server=ntp.domain.com

[web:vars]
db_primary_host=db1.domain.com
db_secondary_host=db2.domain.com
```

- �O�o���̒�`

  - �f�B���N�g���\��

    - playbook��������inventory�t�@�C���̂���f�B���N�g�����牺�L�t�@�C����T���B

    ```
    
    ./host_vars/
	host1    # or host1.yml
	dbhost   # or dbhost.yml
    ./group_vars/
	production  # or *.yml
	staging
    ```

  - file�̓��e

    - host1 / dbhost
```
ansible_ssh_host: 1.2.3.4
ansible_ssh_port: 22
```

     - production / staging

```
db_primary_server: host1
db_secondary_server: host2
#
# playbook�ł̎Q�ƕ��@ : {{ db_primary_server }}
#
```

```
	- notes: group_vars�͎����`���ł���`�\
```
db:
  primary_server: host1
  secondary_server: host2
#
# playbook�ł̎Q�ƕ��@�F{{ db.primary_server }}
#
```

### dynamic inventory

- ���I�C���x���g���X�N���v�g�̎d�l

  - output
    �P��JSON�I�u�W�F�N�g

  - option

    - --host=<hostname>
      �z�X�g�̏ڍׂ�Ԃ�

      OUTPUT:
      
      { "ansible_ssh_host": "1.2.3.4", "ansible_ssh_port": 22,
        "ansible_ssh_user": "vagrant" }
	
    - --list
      �O���[�v�̃��X�g��Ԃ�

      OUTPUT:
      
      { "prod": [ "host1", "host2", ...],
        "staging": [ "host1", "host2", ...],
	"dev": [ "host1", "host2", ...],
	"db": [ "host1", "host2", ...]
      }

# tips

#### Syntax Error while loading YAML. mapping values are not allowed in this context

- error
  - ������̒��ɁF������ƃG���[�ɂȂ�B
```
ERROR! Syntax Error while loading YAML.
  mapping values are not allowed in this context

      debug: msg="stdout: {{ results.stdout }}"
                        ^ here
```

- ����
  - �F�R�����������YAML�ł̕ϐ���`�ƔF�����Ă��܂��G���[�ƂȂ�B

- FIX
  - �܂肽���݃X�^�C���ɕύX����B
```
      debug: >
        msg="stdout: {{ results.stdout }}"
```

#### vault ( encrypt password )

  - ���p���@�T�v
  
    - �ϐ��t�@�C����ansible-vault�R�}���h�ňÍ�������B
    - ���̂Ƃ��Avault password���w�肷��B
    - vault password���w�肵��ansible-playbook�����s����B
      - vault password���w����@����������B�ڍׂ͉��L�Q��

  - vault password�w����@�T�v

    - ���s���ɖ������: --ask-vault-pass
    
    - vault password���L�ڂ����t�@�C���𗘗p
      - �����F	  --vault-password-file=<filename>
      - ���ϐ��F ANSIBLE_VAULT_PASSWORD_FILE=vpassfile
      - ansible.cfg�t�@�C���ݒ�
      
    - vault password�����ϐ��ɐݒ�@����BEST����

    - vault password��public key�𗘗p������@�@����BEST����


  - prepare
```
$ cat vars.org
ansible_become_pass: password
$ cp vars.org vars

$ ansible-vault encrypt vars
New Vault password: <<enter vault password>>

$ cat vars
$ANSIBLE_VAULT;1.1;AES256
346131353662353936333436383936...

$ cat playbook.yml
  ...
  vars_files:
    - vars
  ...
```

  - vault password�w����@�ڍ�

  - how to use : 1
```
$ ansible-playbook playbook.yml --ask-vault-pass
Vault password: <<< enter vault password
```

  - how to use : 2
```
$ cat vpassfile
<<vault password>>

$ ansible-playbook playbook.yml --vault-password-file=vpassfile
```

  - how to use : 3
```
$ export ANSIBLE_VAULT_PASSWORD_FILE=vpassfile
$ ansible-playbook playbook.yml
```

  - how to use : 4
```
$ cat ansible.cfg
[defaults]
vault_password_file=pass

$ ansible-playbook playbook.yml
```

  - how to use : 5 <<<<< BEST ! >>>>>
```
$ export ANSIBLE_VAULT_PASSWORD_FILE=vpass.sh
$ cat vpass.sh
#! /bin/sh
echo "$VPASSWORD"
$ export VPASSWORD=<<vault password>>
$ ansible-playbook playbook.yml
```

  - how to use : 6 <<<<< BEST ! >>>>>
```
$ ansible-vault --vault-password-file=~/.ssh/id_rsa.pub encrypt vars
$ export ANSIBLE_VAULT_PASSWORD_FILE=~/.ssh/id_rsa.pub
$ ansible-playbook playbook.yml
```

#### specify host by command line

```
ansible-playbook playbook.yml --limit "host1"
ansible-playbook playbook.yml --limit "host1,host2"
ansible-playbook playbook.yml --limit 'group1'
ansible-playbook playbook.yml --limit 'all:!host1'
```
- localhost

```
ansible-playbook playbook.yml --connection=local
#-----------------------------------------------------
# CAUTION:
#   ansible���F������host�S�Ă�localhost�Ŏ��s�����I
```

```
- hosts: 127.0.0.1
  connection: local
```

```
# inventory file
localhost ansible_connection=local
#
remote1	  ansible_connection=ssh ansible_user=mpdehaan
```

```
# host_vars / group_vars file
ansible_connection: local
localhost              ansible_connection=local
```


- show
  ```
  # who all hosts
  ansible-playbook --list-hosts playbook.yml

  # who all tasks
  ansible-playbook --list-tasks playbook.yml

- controle run
  ```
  # step execute
  ansible-playbook --step playbook.yml

  # specify start task
  ansible-playbook --start-at-task="install packages" playbook.yml

  # tags
  ansible-playbook -t foo,bar	       playbook.yml
  ansible-playbook --tags=foo,bar      playbook.yml
  ansible-playbook --skip-tags=ccc,ddd playbook.yml
  ```

- tags

  - play��task�Ɉ�itags�j��t�^����d�g��
  - tags���g���Ď��s�̐��䂪�\

  ```
  - hosts: myserver
    tags:
      - foo
    tasks:
    - name: install
      apt: ...
    - name: run
      command: ...
      tags:
	- bar
	- qqq
  ```

- show all infomation of host

  ```
  # by command line
  ansible <hostname> -m setup

  # or use hostvars in playbook task
  - debug: var=hostvars[inventory_hostname]
  ```

- run with root

  ```
  NEW format:
    - become: yes
  # OLD format:
  #  - sudo : yes
  ```
  
- ignore-errors

  - error���������Ă��������ĈȌ�̏����𑱂���B
  - exsample:
    ```
	  - shell: foo-command --version
	    ignore_errors: True # ��L�̃R�}���h�͑��݂��Ȃ��̂ŃG���[���������邪�A�������Ď��s�𑱂���B

	  - debug: msg="debug"
	  ```
- when

  - ���s���ʂŏ����̎��{�𔻒f����B
  - exsample:
    ```
    - shell: foo-command --version
      register: result
	    ignore_errors: True

	  - debug: msg="debug"
      when: refult|failed
    ```

- cow : ��
  - nocows : OFF
    ```
	  $ cat ansible.cfg
	  [defaults]
	  nocows = 1
	  ```
	  ```
	  ANSIBLE_NOCOWS=1 ansible-playbook -i myinventory.ini myplaybook.yml
	  ```

# directory structure

- Inventory
- playbooks
- role
  - common
  - mysql
  - wildfly

# URLs

- docs: http://docs.ansible.com/ansible/index.html

# debug

- ���O�`�F�b�N

  ```
  $ ansible-playbook -i hosts test.yml --syntax-check
  $ ansible-playbook -i hosts test.yml --list-tasks
  $ ansible-playbook -i hosts test.yml --check        ..... ���s���e���`�F�b�N�iDryRun�j
  $ ansible-playbook -i hosts test.yml --check --diff ..... ���s���e���`�F�b�N�iDryRun�j�{�����[�g�t�@�C���ύX
  ```

- ���s���`�F�b�N

  - $ ansible-playbook -vvv

# FACTS�i�ϐ��j
- �m�[�h�Ɋւ���S�t�@�N�g�̕\��
  - `$ ansible node1 -m setup`
- �m�[�h�Ɋւ���ꕔ�̃t�@�N�g�̕\��
    - `$ ansible node1 -m setup -a 'filter=ansible_eth*'`

# lookup
- ansible�������ϐ��̃f�[�^��l�X�ȃ\�[�X����ǂݎ��d�g��
- lookup�̎��
  - file
  - password
  - pipe
  - env
  - template
  - csvfile
  - dnstxt
  - redis_kv
  - etcd

#### �f�[�^�̈Í���

```
$ ansible-vault **command** file.yml
```

- command
  - encrypt
  - decrypt
  - view
  - create
  - edit
  - rekey

#### Galaxy

- URL: http://docs.ansible.com/ansible/galaxy.html
- https://galaxy.ansible.com/

- Ansible Galaxy is Ansible�fs official community hub for sharing Ansible roles. A role is the Ansible way of bundling automation content and making it reusable.

  - https://galaxy.ansible.com/explore#/
  - https://galaxy.ansible.com/list#/roles

- roles
  - samdoran.jboss-eap : Install Red Hat JBoss EAP
    - https://galaxy.ansible.com/samdoran/jboss-eap/
  - samdoran.java : Install Java
    - https://galaxy.ansible.com/samdoran/java/

- Download Roles
  - You can find the most popular roles and recently added roles on the Explore page, or use Browse Roles to search all roles available on Galaxy.

  - Once you find a role you're interested in, you can download it using the ansible-galaxy command that comes bundled with Ansible.

    ```
    $ ansible-galaxy install username.rolename
    ```

  - Be aware that Ansible downloads roles to ANSIBLE_ROLES_PATH, which defaults to /etc/ansible/roles. You can override this by setting the environment variable in your session, defining roles_path in an ansible.cfg file, or by using the --roles-path option.

  - Here's an example of using --roles-path to install the role into the current directory:
    ```
    $ ansible-galaxy install --roles-path . username.rolename  
    ```

## role
- Playbook �𕪊����ė��p�������߂邽�߂̎d�g��
- files
  - default
    - /etc/ansible/roles/**role_name**
- role�̌Ăяo����
  ```
  $ vi /etc/ansible/site.yml
  ---
  - hosts: all
    roles:
      - httpd
  ```
- **role_name** search path
  - /etc/ansible/roles/<role_name>
  - ./roles/<role_name>
  - ./<role_name>
  - ANSIBLE_ROLES_PATH
  - ansible.cfg
    - roles_path = /foo/../roles_path
    - roles_path = /foo/roles:/var/roloes

- **role_name** directory �\��
  - role_name/
    - files/
      - copy���W���[�����g���Ĕz�u����t�@�C����u��
      - �t�@�C���t�H�[�}�b�g�͔C��
    - templates/
      - template���W���[�����g���Ĕz�u����t�@�C����u��
      - �t�@�C���`���FJinja2�`���i�g���q�u.j2�v�j�̃t�@�C���B�ϐ����g���邱�Ƃ������ł��B�ݒ�t�@�C���̃e���v���[�g��u�����Ƃ������B
    - tasks/main.yml
      - ���s����^�X�N�̋L�q
    - hadlers/main.yml
      - �^�X�N�̎��s�ŕύX���������ꍇ�Ɂi�Ⴆ��httpd���C���X�g�[�����ꂽ�jnotify ����Ăяo�����n���h���������Ă����܂��B
    - vars/main.yml
      - �I�[�o�[���C�h���ׂ��łȂ��ϐ��Q
      - �ϐ��̗D�揇�ʂ̏ڍ� : http://docs.ansible.com/ansible/playbooks_variables.html#variable-precedence-where-should-i-put-a-variable
    - defaults/main.yml
      - ���[���Ŏg���ϐ��̃f�t�H���g�l
      - �D�揇�ʂ���ԒႢ�ϐ�
    - meta/main.yml
      - ���[���̈ˑ��֌W�������Ă����܂��B

## install

#### Control Machine : Ansible Server
- Requirements
  - Phthon 2.6 / 2.7
  - ansible
  - ssh�N���C�A���g�@�\

##### �ˑ��p�b�P�[�W�̃C���X�g�[��
```
$ sudo yum check-update
$ sudo yum install -y gcc libffi-devel python-devel openssl-devel
$ sudo yum groupinstall -y "Development Tools"
```
##### Ansible�C���X�g�[��
```
$ sudo yum install -y epel-release
$ sudo yum install -y sshpass
$ sudo yum install -y ansible
```
##### �o�[�W�����m�F
```
$ ansible --version
ansible 2.2.1.0
  config file = /etc/ansible/ansible.cfg
  configured module search path = Default w/o overrides
```

##### �p�X���[�h������sudo�ł���悤�ɂ���
- �^�[�Q�b�g�m�[�h�ňȉ������s
  ```
  $ sudo visudo
  # �ŏI�s�ɒǋL
  <���[�U��> ALL=(ALL)  NOPASSWD: ALL
  ```

- RHEL

  ```
  yum install -y epel-release
  yum install -y ansible
  -openssh-client (A)
  ```
- ubuntu

  ```
  $ sudo http_proxy="http://proxy:8080" apt-get install software-properties-common
  $ sudo http_proxy="http://proxy:8080" apt-add-repository ppa:ansible/ansible
  $ sudo http_proxy="http://proxy:8080" apt-get update
  $ sudo http_proxy="http://proxy:8080" apt-get install ansible
  ```
#### Managed Node : target server

- Requirements
  - python 2.6 or later
  - ssh�T�[�o�@�\


## config file

- file name : ansible.cfg
  - �ŐV�̃t�@�C���Fhttps://raw.github.com/ansible/ansible/devel/examples/ansible.cfg

- parameter
  - http://docs.ansible.com/ansible/intro_configuration.html

- search order:
  - ANSIBLE_CONFIG(an environament variable which has path of ansible.cfg)
  - ./ansible.cfg(current dir.)
  - $HOME/.ansible.cfg(HOME dir.)
  - /etc/ansible/ansible.cfg

- ansible.cfg sample
  ```
  [defaults]
  hostfile = hosts
  remote_user = vagrant
  private_key_file = /home/vagrant/.ssh/id_rsa
  host_key_checking = False
  ```

| name | default | description ( = is over write option by ansible.cfg [defaults]) |
|------|---------|----|
|ansible_ssh_host | hostanme | |
|ansible_ssh_port | 22 |???@= remote_port of ansible.cfg option |
|ansible_ssh_user | root | = remote_user of ansible.cfg option |
|ansible_ssh_pass | N/A | |
|ansible_connection | smart | SSH(ControlPersist) Python:SSH_client |
|ansible_ssh_private_key_file | N/A | = private_key_file of ansible.cfg option |
|ansible_shell_type | sh | = executable of ansible.cfg option |
|ansible_python_interpreter | /usr/bi/python | |
|ansible_*_interpretere | N/A | Python�C���^�v���^�ȊO�̃C���^�[�v���^�𗘗p����ꍇ�ɁA�C���^�[�v���^�[�̃p�X���w�肷��B�i��F/usr/bin/ruby�j


- sample of hostfile
  - simple : ���Lansible.cfg�̐ݒ��O��Ƃ��Đݒ�
    ```
    node1 ansible_ssh_host=node1
    node2 ansible_ssh_host=node2
    ```
  - full
    ```
    node1  ansible_ssh_host=node1  ansible_ssh_user=vagrant  ansible_ssh_key_file=/home/vagrant/.ssh/id_rsa
    node2  ansible_ssh_host=node2  ansible_ssh_user=vagrant  ansible_ssh_key_file=/home/vagrant/.ssh/id_rsa
    ```


## Ansible : Up and Running

# 1 introduction

- install
  - ??????(centos6)????ansible + ssh??????
  ```
  yum install -y epel-release
  yum install -y ansible
  -openssh-client (A)
  ```

  - ????????(centos6)???python + ssh????

	- openssh-server (B)
	- python???????????????????yum install MySQL-python?mysql?????????????

- ansible??
  - /etc/ansible????????
	- http://www.lnc.jp/2015/03/23/2317.html

```
$ mkdir playbooks
$ cd playbooks
$ vagrant init ubuntu/trusty64
$ vagrant up

$ vagrant ssh
$ vagrant ssh-config
Host default
  HostName 127.0.0.1
  User vagrant
  Port 2222
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
  PasswordAuthentication no
  IdentityFile D:/work/ansible/playbooks/.vagrant/machines/default/virtualbox/private_key
  IdentitiesOnly yes
  LogLevel FATAL

$ ssh vagrant@127.0.0.1 -p 2222 -i /cygdrive/d/work/ansible/playbooks/.vagrant/machines/default/virtualbox/private_key

password : vagrant

```

### tutorial-1

- install
  - �N���C�A���g(centos6)���ɂ́@ansible + ssh�N���C�A���g
  ```
  yum install -y epel-release
  yum install -y ansible
  -openssh-client (A)
  ```

  - �z�X�g�T�[�o�[��(centos6)�ɂ́@python + ssh�T�[�o�[

	- openssh-server (B)
	- python�@�{�@�K�v�ɉ����Ẵ��W���[���̒ǉ��iyum install MySQL-python��mysql�h���C�o�[�̃C���X�g�Ƃ��j

- ansible���s
  - /etc/ansible���ō�Ƃ��܂��B
	- http://www.lnc.jp/2015/03/23/2317.html

- config

	- /etc/ansible/hosts�t�@�C����ҏW

	```
	cd /etc/ansible/
	vi hosts

	[test-servers]
	192.168.99.100:32786
	```

- make yml file

  - http���C���X�g�[������t�@�C�����쐬

```
$ vi simple.yml

- hosts: test-server
  tasks:
    - name: install httpd
      yum: name=httpd disable_gpg_check=no state=installed
```

�� yml�`���Ȃ̂ŃC���f���g�ɂ͒���

- run ansible

```
ansible-playbook -i hosts simple.yml --ask-pass
```

