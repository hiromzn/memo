### tutorial: Galaxy

まずはじめに、Ansible のことを少し説明すると、Ansible には、2つのコマンドがあり、実戦では通常、複数のタスクをまとめて実行する仕組みを持った ansible-playbook コマンド（Ansible Playbook）が使われます。

- ansible
- ansible-playbook


次に、Ansible Playbook を利用するには、2つのファイルが最低限必要

- Inventory
  - 制御するサーバ情報を INI形式で記述した設定ファイル
- Playbook
  - 実行するタスクを YAML形式で記述したファイル

picture : <https://cdn-ak.f.st-hatena.com/images/fotolife/a/akiyoko/20151206/20151206111518.png>

実行するタスクを 1つの Playbookファイルに書くと、どうしても可読性が悪くなってしまいがちなので、
```
  tasks:
    - include: tasks/main.yml
```

として別の Playbookファイルを include してファイルを分割する方法もあるのですが、Ansible のベストプラクティスとして、メインとなる実行ファイルを 1つ用意し、そこから、

```
  roles:
     - mysql
```

という形式で Role（task や handler、template などを纏めて再利用できるようにしたもの）を参照したり、

```
  vars_files:
    - vars/database.yml
```

という形式で Variables（変数の値を設定したファイル）を参照したりすることが多いです。



つまり、Ansible Playbook を効率的に使うには、

Inventory（ホスト情報）
Playbook（実行タスクを記述したメインのファイル）
Role（必要なだけいくつでも）
Variables（変数定義用のファイル）
を揃えればよいということになります。

picture : <https://cdn-ak.f.st-hatena.com/images/fotolife/a/akiyoko/20151206/20151206111528.png>

ここで、Role の作成にはノウハウや試行錯誤が必要で、一から作ろうとすると非常に手間がかかります。実戦で使えるようになるまでには、タスクの書き方をあれこれと参考にしながら覚えていく必要がありますし、ベストプラクティスにもある程度従うことも必要でしょう。たとえ時間をかけて作り上げたとしても、すでにベストプラクティスが世の中に存在していて、車輪の再発明をしてしまったということにもなりかねません。


そこで、Ansible Galaxy の登場です！


Ansible Galaxy には、世界中の Ansible厨 \*1 から寄せられた自信作の Role が GitHub のようにアップされていて、それをパクる再利用することが、なるべく手早く Ansible を実戦投入するための近道となっているのです。



Ansible Galaxy については、以下の記事が分かりやすいと思います。
＜参考＞
Ansible Galaxyを使ってRoleをダウンロードする ｜ Developers.IO


Ansible公式のディレクトリ構成のベストプラクティスもあります。
Best Practices — Ansible Documentation


では実際に、Ansible Galaxy を使ってみましょう。

環境

＜クライアント＞

Mac OS X 10.10.5
ansible 1.9.3

＜サーバ＞

Ubuntu 14.04 LTS (on Vagrant)
IPアドレス : 192.168.33.10

　

今回利用した Role

今回利用したのは、
https://galaxy.ansible.com/detail#/role/509
で、Ubuntuサーバに MySQL をインストールするための Role です。

利用できる変数などの詳細については、README を参照してください。



　

1. ANXS.mysql を Ansible Galaxy からインストール

Ansible Galaxy インストールの基本コマンドは、

ansible-galaxy install <username>.<role> -p <path_to_role>
です。


Ansible Galaxy から、ANXS.mysql の role をインストールします。

$ cd ~/dev/
$ mkdir -p ansible-mysql
$ ansible-galaxy install ANXS.mysql -p ansible-mysql/roles

$ cd ansible-mysql/

　

2. 文字化け対策

Ansible Galaxy を使うという本筋からは外れてしまいますが、ここで、日本語の文字化け対策のために、my.cnf のテンプレートを修正しておきます。\*2
$ vi roles/ANXS.mysql/templates/etc_mysql_my.cnf.j2

＜変更前＞
---
# ssl-key=/etc/mysql/server-key.pem

character_set_server = {{ mysql_character_set_server }}
collation_server = {{ mysql_collation_server }}

[mysqldump]
---

＜変更後＞
---
# ssl-key=/etc/mysql/server-key.pem

character-set-server = {{ mysql_character_set_server }}
collation-server = {{ mysql_collation_server }}

[mysqldump]
---
＜参考＞
MariaDB で日本語を扱う場合の文字コードの設定 | 雪猫ノート



　

3. Inventoryファイルを作成

INI形式でホストの情報を列挙します。今回は、サーバを 1つだけ定義しておきます。

hosts

[db-servers]
192.168.33.10
　

4. Playbookファイルを作成

メインとなる Playbookファイルを 1つ作成します。
「user:」でタスクを実行するユーザを指定し、「hosts:」で Inventory のセクション名、「roles:」で実行する Role、「vars_files:」で Variables ファイルのパスを指定してあげます。Playbook ファイルはたったこれだけで OK です。


site.yml

- hosts: db-servers
  user: vagrant
  vars_files:
    - vars/database.yml
  roles:
    - { role: ANXS.mysql }
　

5. Variablesファイルを作成

こちらの Variablesファイルは、作成しなくても構いません。
作成しなければ、ANXS.mysql で規定されているデフォルト値（こちら を参照）が使われることになります。

```
vars/database.yml

mysql_current_root_password: ''
mysql_root_password: rootpass
mysql_databases:
  - name: myproject
mysql_users:
  - name: myprojectuser
    pass: myprojectuserpass
    priv: "myproject.*:ALL"
```

例えば、上記のように変数設定を行うと、MySQLをインストールするのに合わせて、rootユーザのパスワードを「rootpass」に変更し、以下のデータベースとユーザを作成することができます。

項目	設定内容
データベース名	myproject
データベースユーザ	myprojectuser
データベースユーザパスワード	myprojectuserpass

ちなみに、ディレクトリ構成は以下のようになります。

ansible-mysql
├── hosts
├── roles
│   └── ANXS.mysql
│       ├── LICENSE
│       ├── README.md
│       ├── Vagrantfile
│       ├── defaults
│       │   └── main.yml
│       ├── handlers
│       │   └── main.yml
│       ├── meta
│       │   └── main.yml
│       ├── tasks
│       │   ├── configure.yml
│       │   ├── databases.yml
│       │   ├── install.yml
│       │   ├── main.yml
│       │   ├── monit.yml
│       │   ├── secure.yml
│       │   └── users.yml
│       ├── templates
│       │   ├── etc_monit_conf.d_mysql.j2
│       │   ├── etc_mysql_my.cnf.j2
│       │   └── root_dot_my.cnf.j2
│       ├── test.yml
│       ├── vagrant-inventory
│       └── vars
│           └── debian.yml
├── site.yml
└── vars
    └── database.yml

　

6. Playbook を実行

Vagrant インスタンスに SSHアクセスできるか、確認しておきます。

$ ssh vagrant@192.168.33.10

Inventoryファイルと Playbookファイルを指定して、Playbook を実行します。

$ ansible-playbook -i hosts site.yml --sudo -k -vv


ちなみに、使用したオプションには以下のような意味があります。

オプション	意味
--sudo	sudo で実行
-k	パスワード認証でサーバにアクセスする場合にSSHパスワードを入力できるようにする
-vv	デバッグログを詳細に出す

　

7. 確認

Playbook の実行が完了したら、サーバ側で MySQL のセットアップ内容を確認します。

mysql -u root -p
mysql> show variables like 'char%';
+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | utf8                       |
| character_set_connection | utf8                       |
| character_set_database   | utf8                       |
| character_set_filesystem | binary                     |
| character_set_results    | utf8                       |
| character_set_server     | utf8                       |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
8 rows in set (0.00 sec)

mysql> select * from mysql.user;
+--------------------------+------------------+-------------------------------------------+-------------+-------------+-------------+-------------+-------------+-----------+-------------+---------------+--------------+-----------+------------+-----------------+------------+------------+--------------+------------+-----------------------+------------------+--------------+-----------------+------------------+------------------+----------------+---------------------+--------------------+------------------+------------+--------------+------------------------+----------+------------+-------------+--------------+---------------+-------------+-----------------+----------------------+--------+-----------------------+
| Host                     | User             | Password                                  | Select_priv | Insert_priv | Update_priv | Delete_priv | Create_priv | Drop_priv | Reload_priv | Shutdown_priv | Process_priv | File_priv | Grant_priv | References_priv | Index_priv | Alter_priv | Show_db_priv | Super_priv | Create_tmp_table_priv | Lock_tables_priv | Execute_priv | Repl_slave_priv | Repl_client_priv | Create_view_priv | Show_view_priv | Create_routine_priv | Alter_routine_priv | Create_user_priv | Event_priv | Trigger_priv | Create_tablespace_priv | ssl_type | ssl_cipher | x509_issuer | x509_subject | max_questions | max_updates | max_connections | max_user_connections | plugin | authentication_string |
+--------------------------+------------------+-------------------------------------------+-------------+-------------+-------------+-------------+-------------+-----------+-------------+---------------+--------------+-----------+------------+-----------------+------------+------------+--------------+------------+-----------------------+------------------+--------------+-----------------+------------------+------------------+----------------+---------------------+--------------------+------------------+------------+--------------+------------------------+----------+------------+-------------+--------------+---------------+-------------+-----------------+----------------------+--------+-----------------------+
| localhost                | root             | *3800D13EE735ED411CBC3F23B2A2E19C63CE0BEC | Y           | Y           | Y           | Y           | Y           | Y         | Y           | Y             | Y            | Y         | Y          | Y               | Y          | Y          | Y            | Y          | Y                     | Y                | Y            | Y               | Y                | Y                | Y              | Y                   | Y                  | Y                | Y          | Y            | Y                      |          |            |             |              |             0 |           0 |               0 |                    0 |        |                       |
| vagrant-ubuntu-trusty-64 | root             | *3800D13EE735ED411CBC3F23B2A2E19C63CE0BEC | Y           | Y           | Y           | Y           | Y           | Y         | Y           | Y             | Y            | Y         | Y          | Y               | Y          | Y          | Y            | Y          | Y                     | Y                | Y            | Y               | Y                | Y                | Y              | Y                   | Y                  | Y                | Y          | Y            | Y                      |          |            |             |              |             0 |           0 |               0 |                    0 |        |                       |
| 127.0.0.1                | root             | *3800D13EE735ED411CBC3F23B2A2E19C63CE0BEC | Y           | Y           | Y           | Y           | Y           | Y         | Y           | Y             | Y            | Y         | Y          | Y               | Y          | Y          | Y            | Y          | Y                     | Y                | Y            | Y               | Y                | Y                | Y              | Y                   | Y                  | Y                | Y          | Y            | Y                      |          |            |             |              |             0 |           0 |               0 |                    0 |        |                       |
| ::1                      | root             | *3800D13EE735ED411CBC3F23B2A2E19C63CE0BEC | Y           | Y           | Y           | Y           | Y           | Y         | Y           | Y             | Y            | Y         | Y          | Y               | Y          | Y          | Y            | Y          | Y                     | Y                | Y            | Y               | Y                | Y                | Y              | Y                   | Y                  | Y                | Y          | Y            | Y                      |          |            |             |              |             0 |           0 |               0 |                    0 |        |                       |
| localhost                | debian-sys-maint | *1A50E3F67F834184F18493CB9FDC7B91D746FA38 | Y           | Y           | Y           | Y           | Y           | Y         | Y           | Y             | Y            | Y         | Y          | Y               | Y          | Y          | Y            | Y          | Y                     | Y                | Y            | Y               | Y                | Y                | Y              | Y                   | Y                  | Y                | Y          | Y            | Y                      |          |            |             |              |             0 |           0 |               0 |                    0 |        | NULL                  |
| localhost                | myprojectuser    | *B18A090DBF78EF9F2C5F71986EF462287B0C995D | N           | N           | N           | N           | N           | N         | N           | N             | N            | N         | N          | N               | N          | N          | N            | N          | N                     | N                | N            | N               | N                | N                | N              | N                   | N                  | N                | N          | N            | N                      |          |            |             |              |             0 |           0 |               0 |                    0 |        | NULL                  |
+--------------------------+------------------+-------------------------------------------+-------------+-------------+-------------+-------------+-------------+-----------+-------------+---------------+--------------+-----------+------------+-----------------+------------+------------+--------------+------------+-----------------------+------------------+--------------+-----------------+------------------+------------------+----------------+---------------------+--------------------+------------------+------------+--------------+------------------------+----------+------------+-------------+--------------+---------------+-------------+-----------------+----------------------+--------+-----------------------+
6 rows in set (0.00 sec)

バッチリです。


ちなみに、MySQL をサーバからアンインストールするには、

$ sudo apt-get --purge -y autoremove mysql*
とすれば OK です。これで、失敗しても何度もやり直せますね。

＜参考＞
software installation - How do I uninstall Mysql? - Ask Ubuntu



　

まとめ

一から Playbook や Role を作って育てていくのは結構大変です。
そこで、Ansible Galaxy を使って、完成度の高い Role を再利用してしまえば、あとは、Inventoryファイルとメインとなる Playbookファイルを用意し、オプションで Variablesファイルを調整すれば、すぐにでも Ansible を実戦投入できるようになります。

Ansible Galaxy の Role をサンプルにして、Playbook や Role の書き方を覚えていくというのも、Ansible 初心者の学習方法としてよいのではないかと思った次第です。


ところで、久々に Ansible の記事を書いたのですが、@r_rudi（しろう）さんのサイト
ansibleを使ってみる — そこはかとなく書くよん。
には今回も大変お世話になりました。昔から何度も読ませていただいています。
Ansible 本も出版されているようです。Ansible 初心者でしっかり基礎からやりたいという人にはピッタリな内容の本ですね。
