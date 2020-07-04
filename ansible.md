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

./roles/	# <inventory_fileと同階層

./roles/common/vars/main.yml	# roleに共通した変数の定義

./roles/<role_name>/
./roles/<role_name>/tasks/
./roles/<role_name>/tasks/main.yml
	- include <task1.yml>
./roles/<role_name>/tasks/*.yml
	- name: task1
	  yum: name=httpd state=latest
	  notify: handler★★
./roles/<role_name>/handlers/
./roles/<role_name>/handlers/main.yml
	- include <handler1.yml>
./roles/<role_name>/handlers/*.yml
	- name: handler★★
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

- playbook = playのリスト

- play
  - hostの集合 & taskのリスト

  - name : playの内容
  - become : sudoとしての動作
  - vars : 変数

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

### vars, 変数

##### vars（変数）概要

- varsで定義した変数のスコープは、実行中のホストおよびホストが属するグループ

  - 動作的には、同一グループ内に属する各ホストに対して生成される。
  - hostvars.<hostname>.XXXXという変数が作成されるわけではなく、実行環境に対して作成されると思われる。

- 変数定義の優先順位

  - 同じ名前の複数の変数が異なる場所で定義されている場合特定の順序で上書きされる。
  - 大まかな優先順位：（優先順が高いものからリスト）
    - commandline -e(--extra-vars)：最高順位
    - このリスト以外
    - host_vars / group_vars
    - fact
    - role/defaults/main.yml：最低
   
  - URL: https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#variable-precedence-where-should-i-put-a-variable

  - 優先順位（優先順位が最低から最高の順に並んでいる）
    - command line values (eg “-u user”)
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

- 変数の優先順位：変数はどこに置くべきか？

  - 変数が他の変数をどのように上書きするかについて考えるより、数をどこに置くべきかを知っているほうが重要

  - 基本的なvar優先ルール
    - 「ロールデフォルト」（ロール内のデフォルトフォルダ）に入るものは上書きされる。
    - ホスト変数やインベントリ変数はロールのデフォルトに勝る。
    - ロールのvarsディレクトリにあるものはすべて、名前空間内のその変数の以前のバージョンをオーバーライドする。
    - コマンドラインが優先される。

  - var定義ルール
    - どのセクション内でも、varを再定義すると前のインスタンスが上書きされます。
    - hash_behaviour
      - replace
      - merge
    - グループローディング
      - 親子関係に従います。
      - 同じ「親/子」レベルのグループは、アルファベット順にマージされます。
      	- ansible_group_priorityで上書き可能
	- インベントリソースでのみ設定でき、group_vars /では設定できません。

    - inventoryでのansible_ssh_user設定は、コマンドラインを含む他の全てより優先される。
      - inventory: ansible_ssh_user > commandline: -u <user>
      - inventory: ansible_ssh_user > play/task: remote_user: <user>

    - remote_userをinventoryを含めて上書きしたい場合は、追加の変数を使用する。
    
      - commandline: -e "ansible_user=<user>" > inventory: ansible_ssh_user

  - 変数のスコープ

    - グローバル
      - 設定場所
      	- config
	- 環境変数
	- コマンドライン
    - Play
      - それぞれのplayとそれに含まれる構造体
      - 設定場所：
      	- varsエントリ（vars; vars_files; vars_prompt）
	- ロールデフォルト
	- vars
    - ホスト
      - ホストに直接関連付けられている変数
      - 設定場所
      	- インベントリ
	- include_vars
	- ファクト
	- registered : 登録済みタスク出力

### 再利用可能なPlaybook : role / include / import

- https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse.html

- include / import
  - 複数のPlaybookもしくは、同じPlaybook内での複数回利用で使う。

  - すべてのimport*ステートメントは、プレイブックの解析時に前処理されます。
  - すべてのinclude*ステートメントは、プレイブックの実行中に発生したときに処理されます。


- include_task / import_task
  - 任意の深さで使用可能
  - 変数を渡すことが可能
    ```
    - import/include_task: my_tasks.yml
      vars:
        my_user: foo
    - import/include_task: my_tasks.yml
      vars:
        my_user: bar
    ```
  - 静的と動的を混在させることはできますが、プレイブックの診断が難しいバグにつながる可能性があるため、お勧めできません。
  - importおよびincludeに変数を渡すためのkey = value構文は推奨されていません。代わりにYAML vars：を使用してください。
  - インクルードとインポートはhandler：セクションでも使用できます。

- role
  - タスク以上のものを一緒にパッケージ化することが可能
  - 変数、ハンドラ、あるいはモジュールや他のプラグインさえも含むことができる。
  - include / import とは異なり、ロールはAnsible Galaxyを介してアップロードおよび共有することも可能。

- dynamic / static
  - Ansible 2.0から、dynamic include が導入された。
  - Ansible 2.1から、include を強制的に静的にする機能が導入された。
  - Ansible 2.4から、include と importが導入された。

  - import*（import_playbook、import_tasksなど）を使用すると、静的になる。
  - include*（include_tasks、include_roleなど）を使用すると、動的になる。

  - 違い
    - Ansibleは、Playbookの解析中にすべての静的インポートを前処理します。
    - dynamic includeは、そのタスクが検出された時点で実行時に処理されます。

    - tags や条件付きステートメント（when :)など、Ansibleタスクオプションに関しては：

    - 静的インポートの場合、親タスクオプションはインポート内に含まれるすべての子タスクにコピーされます。
    - 動的インクルードの場合、タスクオプションは評価時に動的タスクにのみ適用され、子タスクにはコピーされません。

    - 注意 : role
      - Ansible 2.3より前
      	- ロールは常に、role: オプションを介して静的に含まれることで、他のどのプレイタスクよりも前に常に最初に実行されました。
      - Ansible 2.3から
      	- ロールを他のタスクと一緒にインラインで実行できるようにするためにinclude_roleオプションが導入されました。

- include vs import

  - include*ステートメント
  
    - 主な利点はループ
    - ループをインクルードとともに使用すると、含まれるタスクまたはロールはループ内の各項目に対して1回実行されます。

  - include*の制限

    - 動的インクルード内にのみ存在するタグは--list-tagsの出力に表示されません。
    - 動的インクルード内にのみ存在するタスクは--list-tasksの出力に表示されません。
    - notifyを使って、動的インクルード内から来るハンドラ名を起動することはできません（下記の注を参照）。
    - 動的インクルード内のタスクから実行を開始するために--start-at-taskを使用することはできません。

  - import*の制限
    
    - 上記のように、ループはインポートではまったく使用できません。
    - ターゲットファイルまたはロール名に変数を使用する場合、インベントリソースからの変数（ホスト/グループ変数など）は使用できません。

  - 注意 : 動的タスクへのnotifyの使用に関して
  
    - 動的インクルード自体を起動することは可能
    - これにより、インクルード内のすべてのタスクが実行されます。
    
##### defined vars

- hostvars
  - 全てのホストに対して定義された全ての変数を含む辞書
  - ホスト名がキーになっているのでfactとして参照できる。
    ```
    {{ hostvars['db.exsample.com'].ansible_eth1.ipv4.address }}
    ```
　- 現在のホストに関する全情報(fact)の表示
    ```
    # playbook task
    - debug: var=hostvars[inventory_hostname]
    ```

- inventory_hostname
  - ansibleが知っている現在のホスト名

- group_names
  - 現在のホストがグループになっている全グループ名

- groups
  - グループ名がキーで、メンバーのホストが値とする辞書
  - ex) {"all":[...], "web":[...], ...}

- play_hosts
  - 現在play中でアクティブになっているインベントリのホスト名のリスト

- ansible_version
  - ex) 

##### tasks

- set_fact
  - factを設定・追加するタスク（新変数を定義）

```
  - set_fact: ansp={{ snap_result.stdout }}
```
##### how to use vars

- 変数の定義：playbook内で定義

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

- 変数の定義：ファイルでの定義

```
# playbookの記述
  vars_files:
    - myvars.yml

# myvars.yml
key_file: /etc/...
conf_file: /etc/xx.conf
server_name: node01
```

- 変数の値の表示

```
# print var
- debug: var=<var_name>
```

- タスク実行結果の変数への登録(register)

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

- 行の折り返し（複数行での表記）

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

- 定義形式
  - INI-like
  - YAML

- default group
  - ２つのデフォルトグループがある。
    - all
      - 全てのホストが属するグループ
    - ungrouped
      - 特定のグループに属していないホストが属するグループ
  - 全てのホストは、少なくとも２つのグループの属する。
    - all and ungrouped
    - all and 特定のグループ（１つ以上）

- group
  - childlen group
    - 子グループのメンバーであるホストは、自動的に親グループのメンバーになる。
    - 子グループの変数は、親グループの変数よりも優先される（上書きされます）。
    - グループは複数の親と子を持つことができますが、循環関係はできない。
    - ホストは複数のグループに属することもできるが、ホストのインスタンスは1つしかなく、複数のグループからのデータがマージされる。

- ホストとグループ固有のデータの分割
  - Ansibleでは、メインインベントリファイルに変数を格納しないことをお勧めします。
  - ホスト変数とグループ変数は別々のファイルに格納可能。
  - 変数ファイルはYAML形式（拡張子：.yml, .yaml, .json, または拡張子なし）

- group_vars/ and host_vars/

  - 格納ディレクトリ：　group_vars/ and host_vars/
    - playbookディレクトリまたはinventoryディレクトリに配置可能。
    - 両方のパスが存在する場合、playbookディレクトリの変数は、inventoryディレクトリに設定されている変数を上書きします。

  - 管理方法
    - インベントリファイルと変数をGitリポジトリに保存することが推奨
    - インベントリとホスト変数の変更を追跡可能になるため。

- file and directory structure

  - one file

```
# ./<inventory_file>

<hostname1>
<hostname2>

[<group_name1>]
<hostname3>	<var1>=111 <var2>=222	# ホスト変数
<hostname4>	<var1>=111 <var2>=222	# ホスト変数

[<group_name3>:children]
<group_name1>
<group_name2>

[<group_name3>:vars]
<var1>=111	# グループ変数
<var2>=222	# グループ変数

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
./group_vars/prodction_group	# production_group変数の定義ファイル
./group_vars/develop_group
./group_vars/all
./group_vars/ungrouped
./host_vars/host1		# host1変数の定義ファイル
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
# ==> group変数を複数のファイルに分割する場合。
#     ファイルは辞書順に読み込まれる。
#
./<inventory_file>
./group_vars/prodction_group/01_core	# production_group変数を分割
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
- 変数のマージ
  - デフォルト
    - プレイが実行される前に変数は特定のホストにマージ/フラット化される
    - Ansibleはホストとタスクに集中しているため、グループはインベントリとホストのマッチング以外には利用されない。
    - Ansibleはグループやホストに定義されているものも含めて変数を上書きする。
  - デフォルトを変更するには、hash_merge 設定を参照
  - 変数の優先順位（最低から最高、最初のものは後にものに上書きされる。）

    - all (すべてのグループ)
    - 親グループ
    - 子グループ
    - ホスト

  - 同じ親子レベルのグループがマージされると、アルファベット順に行われ、最後にロードされたグループが前のグループを上書きします。 

  - Ansibleバージョン2.4以降
  - グループ変数ansible_group_priorityを使用して、同じレベルのグループのマージ順を変更可能（親子順序が解決された後）
  - 数字が大きいほど、後でマージされ、優先順位が高くなります。 
  - 設定されていない場合、この変数のデフォルトは1

  - 以下の例では、a_groupに高い優先順位を与えるため、結果はtestvar == aになる。
  
    ```
    a_group：
	testvar：a
    	ansible_group_priority：10
    b_group：
	testvar：b
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
      - 設定する場合は、plain textは使わずvaultで設定する。
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
# COMMENT : mytest1とmytest2は異なるホストとして扱われる。
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
# nested group : 多重定義も可能
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
# nested group : 多重定義も可能
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

- hostおよびgroupに対する変数の定義をまとめて実施する方法

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

- 外出しの定義

  - ディレクトリ構成

    - playbookもしくはinventoryファイルのあるディレクトリから下記ファイルを探す。

    ```
    
    ./host_vars/
	host1    # or host1.yml
	dbhost   # or dbhost.yml
    ./group_vars/
	production  # or *.yml
	staging
    ```

  - fileの内容

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
# playbookでの参照方法 : {{ db_primary_server }}
#
```

```
	- notes: group_varsは辞書形式でも定義可能
```
db:
  primary_server: host1
  secondary_server: host2
#
# playbookでの参照方法：{{ db.primary_server }}
#
```

### dynamic inventory

- 動的インベントリスクリプトの仕様

  - output
    １つのJSONオブジェクト

  - option

    - --host=<hostname>
      ホストの詳細を返す

      OUTPUT:
      
      { "ansible_ssh_host": "1.2.3.4", "ansible_ssh_port": 22,
        "ansible_ssh_user": "vagrant" }
	
    - --list
      グループのリストを返す

      OUTPUT:
      
      { "prod": [ "host1", "host2", ...],
        "staging": [ "host1", "host2", ...],
	"dev": [ "host1", "host2", ...],
	"db": [ "host1", "host2", ...]
      }

# tips

#### Syntax Error while loading YAML. mapping values are not allowed in this context

- error
  - 文字列の中に：があるとエラーになる。
```
ERROR! Syntax Error while loading YAML.
  mapping values are not allowed in this context

      debug: msg="stdout: {{ results.stdout }}"
                        ^ here
```

- 原因
  - ：コロンがあるとYAMLでの変数定義と認識してしまいエラーとなる。

- FIX
  - 折りたたみスタイルに変更する。
```
      debug: >
        msg="stdout: {{ results.stdout }}"
```

#### vault ( encrypt password )

  - 利用方法概要
  
    - 変数ファイルをansible-vaultコマンドで暗号化する。
    - このとき、vault passwordを指定する。
    - vault passwordを指定してansible-playbookを実行する。
      - vault passwordを指定方法が複数ある。詳細は下記参照

  - vault password指定方法概要

    - 実行時に毎回入力: --ask-vault-pass
    
    - vault passwordを記載したファイルを利用
      - 引数：	  --vault-password-file=<filename>
      - 環境変数： ANSIBLE_VAULT_PASSWORD_FILE=vpassfile
      - ansible.cfgファイル設定
      
    - vault passwordを環境変数に設定　＜＜BEST＞＞

    - vault passwordにpublic keyを利用する方法　＜＜BEST＞＞


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

  - vault password指定方法詳細

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
#   ansibleが認識するhost全てがlocalhostで実行される！
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

  - playやtaskに印（tags）を付与する仕組み
  - tagsを使って実行の制御が可能

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

  - errorが発生しても無視して以後の処理を続ける。
  - exsample:
    ```
	  - shell: foo-command --version
	    ignore_errors: True # 上記のコマンドは存在しないのでエラーが発生するが、無視して実行を続ける。

	  - debug: msg="debug"
	  ```
- when

  - 実行結果で処理の実施を判断する。
  - exsample:
    ```
    - shell: foo-command --version
      register: result
	    ignore_errors: True

	  - debug: msg="debug"
      when: refult|failed
    ```

- cow : 牛
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

- 事前チェック

  ```
  $ ansible-playbook -i hosts test.yml --syntax-check
  $ ansible-playbook -i hosts test.yml --list-tasks
  $ ansible-playbook -i hosts test.yml --check        ..... 実行時影響チェック（DryRun）
  $ ansible-playbook -i hosts test.yml --check --diff ..... 実行時影響チェック（DryRun）＋リモートファイル変更
  ```

- 実行時チェック

  - $ ansible-playbook -vvv

# FACTS（変数）
- ノードに関する全ファクトの表示
  - `$ ansible node1 -m setup`
- ノードに関する一部のファクトの表示
    - `$ ansible node1 -m setup -a 'filter=ansible_eth*'`

# lookup
- ansibleが扱う変数のデータを様々なソースから読み取る仕組み
- lookupの種類
  - file
  - password
  - pipe
  - env
  - template
  - csvfile
  - dnstxt
  - redis_kv
  - etcd

#### データの暗号化

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

- Ansible Galaxy is Ansible’s official community hub for sharing Ansible roles. A role is the Ansible way of bundling automation content and making it reusable.

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
- Playbook を分割し再利用性を高めるための仕組み
- files
  - default
    - /etc/ansible/roles/**role_name**
- roleの呼び出し方
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

- **role_name** directory 構成
  - role_name/
    - files/
      - copyモジュールを使って配置するファイルを置く
      - ファイルフォーマットは任意
    - templates/
      - templateモジュールを使って配置するファイルを置く
      - ファイル形式：Jinja2形式（拡張子「.j2」）のファイル。変数を使えることが特徴です。設定ファイルのテンプレートを置くことが多い。
    - tasks/main.yml
      - 実行するタスクの記述
    - hadlers/main.yml
      - タスクの実行で変更があった場合に（例えばhttpdがインストールされた）notify から呼び出されるハンドラを書いておきます。
    - vars/main.yml
      - オーバーライドすべきでない変数群
      - 変数の優先順位の詳細 : http://docs.ansible.com/ansible/playbooks_variables.html#variable-precedence-where-should-i-put-a-variable
    - defaults/main.yml
      - ロールで使う変数のデフォルト値
      - 優先順位が一番低い変数
    - meta/main.yml
      - ロールの依存関係を書いておきます。

## install

#### Control Machine : Ansible Server
- Requirements
  - Phthon 2.6 / 2.7
  - ansible
  - sshクライアント機能

##### 依存パッケージのインストール
```
$ sudo yum check-update
$ sudo yum install -y gcc libffi-devel python-devel openssl-devel
$ sudo yum groupinstall -y "Development Tools"
```
##### Ansibleインストール
```
$ sudo yum install -y epel-release
$ sudo yum install -y sshpass
$ sudo yum install -y ansible
```
##### バージョン確認
```
$ ansible --version
ansible 2.2.1.0
  config file = /etc/ansible/ansible.cfg
  configured module search path = Default w/o overrides
```

##### パスワード無しでsudoできるようにする
- ターゲットノードで以下を実行
  ```
  $ sudo visudo
  # 最終行に追記
  <ユーザ名> ALL=(ALL)  NOPASSWD: ALL
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
  - sshサーバ機能


## config file

- file name : ansible.cfg
  - 最新のファイル：https://raw.github.com/ansible/ansible/devel/examples/ansible.cfg

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
|ansible_*_interpretere | N/A | Pythonインタプリタ以外のインタープリタを利用する場合に、インタープリターのパスを指定する。（例：/usr/bin/ruby）


- sample of hostfile
  - simple : 下記ansible.cfgの設定を前提として設定
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
  - クライアント(centos6)側には　ansible + sshクライアント
  ```
  yum install -y epel-release
  yum install -y ansible
  -openssh-client (A)
  ```

  - ホストサーバー側(centos6)には　python + sshサーバー

	- openssh-server (B)
	- python　＋　必要に応じてのモジュールの追加（yum install MySQL-pythonでmysqlドライバーのインストとか）

- ansible実行
  - /etc/ansible内で作業します。
	- http://www.lnc.jp/2015/03/23/2317.html

- config

	- /etc/ansible/hostsファイルを編集

	```
	cd /etc/ansible/
	vi hosts

	[test-servers]
	192.168.99.100:32786
	```

- make yml file

  - httpをインストールするファイルを作成

```
$ vi simple.yml

- hosts: test-server
  tasks:
    - name: install httpd
      yum: name=httpd disable_gpg_check=no state=installed
```

※ yml形式なのでインデントには注意

- run ansible

```
ansible-playbook -i hosts simple.yml --ask-pass
```

