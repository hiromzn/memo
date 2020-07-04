- Ansible�̊K�w

- �g�D�iOrganization�j

  - �W���u�����s���邽�߂́A�v���W�F�N�g�A�C���x���g���A�F�؏��A�W���u�e���v���[�g
    �Ȃǂ�Ansible Tower �̍\���v�f���܂Ƃ߂ĊǗ�����P�ʁB
  - Ansible Tower �Ǘ��҂͕����̑g�D���Ǘ�����Ƃ����\�}

- �v���W�F�N�g�iProject�j
  - �v���C�u�b�N���Ǘ�����P�ʁB1�̃v���W�F�N�g�ɂ́A1�̃v���C�u�b�N�I�u�W�F�N�g�i���[�J���f�B���N�g���AGithub�A�h���X�Ȃǁj�����蓖�Ă���B

- �C���x���g���iInventory�j
  - ��Ǘ��z�X�g�̃z�X�g����ϐ��Ȃǂ̏���ێ�����B
    - �F�؂Ɋւ�����i���[�U�[����p�X���[�h�j�͕ʊǗ�


# host

- name : hostname or IP_address
- variable:
  - local connection :
    ansible_connection: local
  - remote connection : (default)
    - ansible_connection: ssh
    - ansible_ssh_user: hmizuno
    - ansible_ssh_pass: **password**
    - ansible_sudo_pass: **password**

# tips

- awx log

  �f�t�H���g�ł�awx_task, awx_web�̃R���\�[���Ƀ��O���o�͂����B
  
  ���O���e�m�F���@
    �ȉ��ǂ��炩�̕��@�Ŋm�F����B
	docker�z�X�g�F/var/log/messages
	$ docker logs awx_task

  �ݒ�́A/etc/tower/settings.py�Œ�`

## install

- ��
  - CentOS 7

#### install : tower

- 2019/3/14

- 2�̃p�b�P�[�W������B
  - bundle��
    - 
- https://releases.ansible.com/ansible-tower/setup/ansible-tower-setup-latest.tar.gz
- https://releases.ansible.com/ansible-tower/setup-bundle/ansible-tower-setup-bundle....

#### install : github

- https://github.com/ansible/awx/blob/devel/INSTALL.md

- �K�v�ȃp�b�P�[�W�̃C���X�g�[��

```
$ sudo yum check-update
$ sudo yum check-update
$ sudo yum check-update

$ sudo yum -y install git ansible docker python-docker-py
```

##### docker daemon �̋N��

```
$ sudo systemctl start docker
$ sudo systemctl enable docker
```

##### AWX ���|�W�g���̃C���X�g�[��

```
$ git clone https://github.com/ansible/awx.git
$ cd awx/installer
$ sudo ansible-playbook -i inventory install.yml
```

- proxy setting for docker

```
# vi /etc/sysconfig/docker
HTTPS_PROXY=http://web-proxy.jp.hpecorp.net:8080
HTTP_PROXY=http://web-proxy.jp.hpecorp.net:8080

# This file is referenced from /usr/lib/systemd/system/docker.service 
```

##### run AWX

```
access localhost/
```

- admin / password

##### TIPS

- projcts directory

  - WARNING: There are no available playbook directories in /var/lib/awx/projects. Either that directory is empty, or all of the contents are already assigned to other projects. Create a new directory there and make sure the playbook files can be read by the "awx" system user, or have AWX directly retrieve your playbooks from source control using the SCM Type option above.

  - �����F
    �R���e�i���Ŏ��s�����ꍇ�Aawx-task��awx-web�̗��R���e�i�Ԃ�/var/lib/awx/projects�f�B���N�g����
    ���L�ł��Ă��Ȃ����Ƃ����B

  - �������@
    �z�X�g�̓��f�B���N�g�����e�R���e�i�Ń}�E���g���āA���L����B

    ```
    [root@localhost awx]# cd installer/
    [root@localhost installer]# vi inventory
    # AWX project data folder. If you need access to the location where AWX stores the projects
    # it manages from the docker host, you can set this to turn it into a volume for the container.
    #project_data_dir=/var/lib/awx/projects
    project_data_dir=/var/lib/awx/projects

    [root@localhost installer]# ansible-playbook install.yml -i inventory
    ```

  - info
    - https://sky-joker.tech/2018/03/21/awx%E3%81%ABvarlibawxprojects%E3%81%8C%E3%81%AA%E3%81%84%E6%99%82%E3%81%AE%E5%AF%BE%E5%87%A6/
    - https://github.com/ansible/awx/issues/229
    - https://github.com/ansible/awx/issues/239
