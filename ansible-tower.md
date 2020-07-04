- Ansibleの階層

- 組織（Organization）

  - ジョブを実行するための、プロジェクト、インベントリ、認証情報、ジョブテンプレート
    などのAnsible Tower の構成要素をまとめて管理する単位。
  - Ansible Tower 管理者は複数の組織を管理するという構図

- プロジェクト（Project）
  - プレイブックを管理する単位。1つのプロジェクトには、1つのプレイブックオブジェクト（ローカルディレクトリ、Githubアドレスなど）が割り当てられる。

- インベントリ（Inventory）
  - 被管理ホストのホスト名や変数などの情報を保持する。
    - 認証に関する情報（ユーザー名やパスワード）は別管理


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

  デフォルトではawx_task, awx_webのコンソールにログが出力される。
  
  ログ内容確認方法
    以下どちらかの方法で確認する。
	dockerホスト：/var/log/messages
	$ docker logs awx_task

  設定は、/etc/tower/settings.pyで定義

## install

- 環境
  - CentOS 7

#### install : tower

- 2019/3/14

- 2つのパッケージがある。
  - bundle版
    - 
- https://releases.ansible.com/ansible-tower/setup/ansible-tower-setup-latest.tar.gz
- https://releases.ansible.com/ansible-tower/setup-bundle/ansible-tower-setup-bundle....

#### install : github

- https://github.com/ansible/awx/blob/devel/INSTALL.md

- 必要なパッケージのインストール

```
$ sudo yum check-update
$ sudo yum check-update
$ sudo yum check-update

$ sudo yum -y install git ansible docker python-docker-py
```

##### docker daemon の起動

```
$ sudo systemctl start docker
$ sudo systemctl enable docker
```

##### AWX リポジトリのインストール

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

  - 原因：
    コンテナ環境で実行した場合、awx-taskとawx-webの両コンテナ間で/var/lib/awx/projectsディレクトリが
    共有できていないことが問題。

  - 解決方法
    ホストの同ディレクトリを各コンテナでマウントして、共有する。

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
