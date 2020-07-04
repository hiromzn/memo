## transacton

- recordに対して、xidと値のペアが格納されている。
- xminに入っている。
- xidは、4バイト
	- このため、xidは循環してしまうので、対応が必要

## vacume : バキューム

- AUTOバキュームが設定されていないくても、強制的にバキューム処理が走る。
- フリーズ処理
	- ものすごく古いトランザクションをxidを2に設定する。
	- コミットされたレコードに残っている古いxidは必要ないので、2に変更されたりする。（バージョンによって詳細な動作が異なる）
	- この処理は、AUTOバキュームの中で実施されている。
	- バキュームに-freezeオプションをつけると実施される。

## pg_hba.conf reload

```
su - postgres
/usr/bin/pg_ctl reload

Option 2: Using SQL

SELECT pg_reload_conf();
```

## download
- RUN:
	- http://www.enterprisedb.com/products-services-training/pgdownload
		  postgresql-9.4.1-1-linux-x64.run
- RPM:
	- http://yum.pgrpms.org/9.4/redhat/rhel-7Server-x86_64/
- Source:
	- http://www.postgresql.org/ftp/source/v9.4.1/

## install

  ```
  # groupadd postgres
  # useradd -g postgres postgres
  # password postgres

  # ./postgresql-9.6.1-1-linux-x64.run

  # su - postgres
  $ vi .bashrc
  . /opt/PostgreSQL/9.4/pg_env.sh
  ```
## 基本構造

- PostgreSQLデータベースクラスタには、複数のデータベースを含む。
	- ユーザおよびグループはクラスタ全体で共有される。
	- データは複数のデータベース間で共有されない。
- クライアントは、接続時に指定した単一のデータベース内のデータにしかアクセスできない。
- データベースには、複数のスキーマを含む
- スキーマにはテーブルを含む
	- スキーマには、データ型、関数および演算子などの他の名前付きオブジェクトも含む。
	- 同じオブジェクト名を異なるスキーマで使用可能
		- 例: schema1とmyschemaの両方のスキーマにmytableというテーブルを定義可能
		- ユーザは、権限さえ持っていれば接続しているデータベース内のどのスキーマのオブジェクトにでもアクセスすることができる。

### **重要**
- SQL分の名前の**大文字と小文字を区別**するときは、**""で括る**
  ```
  > CREATE ROLE "BIGsmall" WITH LOGIN INHERIT PASSWORD 'jw8s0F4';
  ```

#### trouble shoot

- ERROR: permission denied to set parameter "session_replication_role"
  ```
  LINE:5: init cluster (id=1,comment='Master');

  <stdin>:5: PGRES_FATAL_ERROR SET datestyle TO 'ISO'; SET session_replication_role TO local;  - ERROR:  permission denied to set parameter "session_replication_role"
  Unable to set session configuration parameters
  ```
  - FIX:
    - クラスタを構成するノードの設定のユーザ名をmasteruserからpostgresに変更したら解決した。
      ```
      node 1 admin conninfo = 'dbname=$DB1 host=$HOST1 port=$PORT1 user=$USER1 password=$PASS1';
node 2 admin conninfo = 'dbname=$DB2 host=$HOST2 port=$PORT2 user=$USER2 password=$PASS2';
      ```
    - masteruserのロールは以下
      - LOGiN, INHERIT

```
postgres=# create role "masteruser" with login inherit password 'masterpass';CREATE ROLE
postgres=# create role "slaveuser" with login inherit password 'slavepass';
postgres=# create database slavedb owner slaveuser;
postgres=# create database masterdb owner masteruser;

$ psql -d masterdb -U masteruser
masterdb=> create table mastertbl (id serial primary key, comment text, ins_time timestamp default current_timestamp );
masterdb=> \d
                 List of relations
 Schema |       Name       |   Type   |   Owner
--------+------------------+----------+------------
 public | mastertbl        | table    | masteruser
 public | mastertbl_id_seq | sequence | masteruser
(2 rows)

$ psql -d slavedb -U slaveuser
slavedb=> create table slavetbl (id serial primary key, comment text, ins_time timestamp default current_timestamp );
slavedb=> \d
                List of relations
 Schema |      Name       |   Type   |   Owner
--------+-----------------+----------+-----------
 public | slavetbl        | table    | slaveuser
 public | slavetbl_id_seq | sequence | slaveuser
(2 rows)


```


## psql command

```
$ psql

$ psql -f sql_command_script
$ psql -U username -d db_name -h hostname

$ psql mydb

> \c <database> ..... 接続データベースに接続（他のDBに変更）
> \l .... database一覧表示

> \dn　... スキーマ一覧を表示

> \dt .... table一覧表示
> \d ..... table一覧

> SELECT version();

> \h .... help
> \q .... quit

> \i sql_command_script ..... execute sql script

> \du .... role/user 一覧表示

> \dp (public|my_schema) .... publicとmy_schemaの情報表示
```
### execut by no password

- 方法
  - $HOME/.pgpass
  - PGPASSWD環境変数

- .pgpass
  - format:
	```
	hostname:port:database:username:password
	```
  - example:
	```
    $ id
    postgres
    $ chmod 600 $HOME/.pgpass
    $ cat $HOME/.pgpass
    localhost:5432:*:postgres:<password>
    $ ll /home/postgres/.pgpass
    -rw-------. 1 postgres postgres 35  1月 14 12:10 /home/postgres/.pgpass
    ```

## parameter

```
> show all;
```

## LOG

- postgresql.conf
  ```
  #log_destination = 'stderr'
  redirect_stderr = on
  log_directory = 'pg_log'
  log_filename = 'postgresql-%a.log'
  log_truncate_on_rotation = on
  log_rotation_age = 1440
  log_rotation_size = 0
  #log_connections = off
  #log_disconnections = off
  # log for ALL
  #log_statement = 'none'
  ```

## db: databease

```
$ createdb mydb
$ dropdb mydb

> \l ... list DATABASE

> CREATE DATABASE name;
$ createdb dbname

> CREATE DATABASE dbname OWNER rolename;

$ createdb -O rolename dbname

DROP DATABASE name;
$ dropdb dbname
```

- テーブル定義の完全な情報確認
  ```
  $ pg_dump database_name -U postgres -s -t table_name
  ...SQL分で表示...
  ```
## schema

- 各データベースに含まれるデフォルトのスキーマ
	- public
	- pg_catalog .... システムテーブルと全ての組み込みデータ型、関数および演算子を含む

- 検索パス
	- SHOW search_path; ... 検索パスの確認
		- default:  "$user",public
	- search_pathには、複数のスキーマを指定可能
	- サーチロジック
		- 作成：最初に存在するスキーマ内にオブジェクトを作成する。
		- 検索：最初のスキーマから順にオブジェクトを検索していく。
	- SET search_path TO myschema,public; .... サーチパスの設定

```
> \dn　... スキーマ一覧を表示
> create schema pguser01 authorization pguser01;　... pguser01 スキーマを作成
> drop schema public;　... public スキーマを削除
> select current_schema; ... 現在のスキーマを確認
> set search_path to <schema_name>; ... スキーマを変更
> show search_path; ... search_pathのデフォルト
```

## table

- sample

```
create table member(
		id char(15),
		log text
		);

create table member(
		id char(10) primary key,
		flg int2,
		count int2 default 0,
		accessdate timestamp,
		updatedate timestamp default current_timestamp not null,
		uagent text
		);

alter table <table_name>
```

## select

```
select *
  from mytable
  where ins_time > '2017-04-28 16:06:28'
  order by ins_time desc;

select *
  from mytable
  order by ins_time desc limit 3;

```

## role/user

```
$ createuser name
$ dropuser name

> select current_user; ... 現在のユーザを表示
> \du ... ユーザ一覧表示
> select * from pg_user; .... ユーザ情報一覧表示
> CREATE ROLE name;
> CREATE ROLE name WITH ADMIN;

> CREATE ROLE miriam WITH LOGIN INHERIT PASSWORD 'jw8s0F4';

- 大文字と小文字を区別するときは、""で括る！
> CREATE ROLE "BIGsmall" WITH LOGIN INHERIT PASSWORD 'jw8s0F4';

> ALTER USER ユーザ名 WITH PASSWORD '新しいパスワード'; ... password設定

> DROP ROLE name;

> CREATE USER foo WITH PASSWORD 'jw8s0F4';

> GRANT 権限, ... ON テーブル名, ... on <table> TO ロール名, ...;　... 権限付与
> REVOKE 権限, ... ON テーブル名, ... on <table> from ロール名, ...;　... 権限剥奪
  権限：
  	SELECT, INSERT, UPDATE, DELETE, REFERENCES, TRIGGER
  	ALL

ユーザーの切り替え
> \connect - <USER_NAME>

スーパーユーザーへ変更
> alter role <USER_NAME> with creatural superuser;

スーパーユーザー権限剥奪
> alter role <USER_NAME> with creatural nosuperuser;
```

- 既存のロール群を求める
  - `> SELECT rolname FROM pg_roles;`

- ロールの表示
  - `> \du`

```
ALTER ROLE role_name [ [ WITH ] <option> [ ... ] ]

<option>:

      SUPERUSER | NOSUPERUSER
    | CREATEDB | NOCREATEDB
    | CREATEROLE | NOCREATEROLE
    | CREATEUSER | NOCREATEUSER <<非推奨>>
    | INHERIT | NOINHERIT
    | LOGIN | NOLOGIN
    | REPLICATION | NOREPLICATION
    | CONNECTION LIMIT connlimit
    | [ ENCRYPTED | UNENCRYPTED ] PASSWORD 'password'
    | VALID UNTIL 'timestamp'
```

## tools

PostgreSQL 9.3以降用：運用環境でも利用できる、WorkLoad Analyzer

	POSTGRESQL WORKLOAD ANALYZER
	http://dalibo.github.io/powa/

	PostgreSQLのワークロード解析ツール「PoWA 1.2」リリース
	http://sourceforge.jp/magazine/14/10/31/063400

## file structure

DATA = /usr/local/pgsql/data

|file|説明|
|-------------|------|
| PG_VERSION  | PostgreSQLのバージョン番号ファイル |
| pg_hba.conf |  ホスト認証設定ファイル |
| pg_ident.conf | identによる認証ファイル |
| postgresql.conf | 実行時パラメータ設定ファイル |
| postmaster.opts | 起動オプション記録 |
| base/ | データ領域(データベースのデータを格納する) |
| global/ |  (コントロールファイルやパスワードファイルなど)共通オブジェクトの保存ディレクトリ
| pg_clog/  | ミットログをおくディレクトリ(コミットログはすべてのトランザクションのコミット状態を記録す る) |
| pg_xlog/ |  WALログ(トランザクションログ)をおくディレクトリ |
| pg_subtrans/ | サクブトランザションの状態を記録する(バージョン8.0から) |
| pg_tblspc/ | テーブルスペースへのシンボリックリンクを記録する(バージョン8.0から)
| pg_twophase/ | 準備されたトランザクション(2相コミット)の状態を記録する(バージョン8.1から)
| pg_multixact/ | マルチトランザクションの状態を記録する。共有行ロックで使用(バージョン8.1から)

#### 参照URL:
###### システム情報関数
  https://www.postgresql.jp/document/9.6/html/functions-info.html
###### システム管理関数
  https://www.postgresql.jp/document/9.6/html/functions-admin.html
###### バックアップ制御関数
  https://www.postgresql.jp/document/9.6/html/functions-admin.html#functions-admin-backup

# WAL / Archive log

- URL: https://www.slideshare.net/suzuki_hironobu/recovery-11

## WAL

- 先行書き込みログ（WAL）：　REDOログのこと
- データベースのデータファイルに行われた全ての変更をLSNで管理してファイルにシリアルに記録する。
- LSN：Log Sequence Number
- file : $DATA/pg_xlog
- WALログは16[Mbyte]のファイル(WALセグメントファイル)に分割される。

## archive

- 使い終わったWALログは「アーカイブログ」として、別ディレクトリに保存される。
- file : example : $DATA/archive
- アーカイブログとは、本来 なら消去されるWALログに他ならない。
- 「継続的アーカイブ」とは、"オンラインバックアップ"のこと。

## CHECKPOINT

- トランザクション処理において、以前に書かれた全ての情報がデータファイルに保存されていることを保証する場所
  - チェックポイントの時刻において、全てのダーティページデータはディスクにフラッシュされ、特殊なチェックポイントレコードがログファイルに書き込まれる。 (変更されたレコードは以前にWALフラッシュされている。)
  - チェックポイント以前になされたデータの変更はディスク上にあることが保証されているので、チェックポイント以前のログセグメントは不要
- クラッシュした時の復旧処理は最後のチェックポイントレコードを見つけ、ログの中でどのレコード（これはredoレコードと呼ばれています）から復旧処理がREDOログ操作を開始すべきかを決定する。

- チェックポイントの処理は、自動的に時々、実行される。
- チェックポイントは、checkpoint_timeout秒が経過するか、または max_wal_size に達するかのどちらかの条件が満たされると開始される。
- デフォルト設定
  - checkpoint_timeout : 5分
  - max_wal_size : 1GB
- 前回のチェックポイント以降書き出すWALがない場合、checkpoint_timeoutが経過したとしても新しいチェックポイントは実施されない。
- WALアーカイブ処理をしている場合は、archive_timeout パラメータでアーカイブ頻度の下限を設定してデータ損失の可能性を限定する。
- CHECKPOINT SQLコマンドで強制的にチェックポイントを実行することも可能

#### checkpoint 起動タイミング

- CHECKPOINTが発生するタイミングは以下

  - 前回のcheckpointから checkpoint_timeout に設定した時間が経過した場合。デフォルトは300秒(5分)
  - バージョン9.4以前
    - checkpoint_segmentsの数 X WALファイルサイズ（16MB）の変更履歴(default:64MB)がWALに書き込まれた場合
    - checkpoint_segments : デフォルト数は3
  - バージョン9.5以後
    - max_wal_size の変更履歴がWALに書き込まれたを超えた場合
    - max_wal_size : デフォルトのサイズは1Gbyte
  - smartモードかfastモードでPostgreSQLサーバを停止した場合
  - 手動でCHECKPOINTコマンドを実行した場合
  - pg_start_backup()関数を実行した時

## operation
- pg_witch_xlog()
  - archiveログを切り替える
  - 次のトランザクションログファイルに移動し、現在のファイルをアーカイブする。
  - 戻り値
    - 完了した現在のトランザクションログファイル内の終了トランザクションログの位置に1を加えたもの
    - 前回のトランザクションログファイルの切り替えからトランザクションログに変化がなければ、
      現在使用中のトランザクションログファイルの開始位置を返す。
	  ```
      postgres=# select pg_switch_xlog();
       pg_switch_xlog
      ----------------
       0/156EDB8
      (1 row)
	  ```

- 現在のWALの書き込み位置の確認

  ```
  postgres=# select pg_current_xlog_location();
  pg_current_xlog_location
  --------------------------
  0/156E860
  (1 row)
  ```

- トランザクションログの位置を表す文字列をファイル名に変換

  ```
  postgres=# select * from pg_xlogfile_name_offset('1/156E860');
          file_name         | file_offset
  --------------------------+-------------
   000000010000000100000001 |     5695584
  (1 row)
  ```


#### archiveの設定（WALアーカイブの有効化）

- postgresql.conf ファイルに以下の設定をする。

  - wal_level = archive（またはarchiveより高いパラメータ）
  - archive_mode = on
  - archive_command = 構成パラメータで使用するシェルコマンドを指定
    - example :
      - `archive_command = 'test ! -f /mnt/server/archivedir/%f && cp %p /mnt/server/archivedir/%f'  # Unix`

## configuration

#### wal_level

- 説明
  - WALへの書き込みレベル

- 値
  - minimal (default)
	- クラッシュまたは即時停止から回復するのに必要な情報のみ書き出す
  - archive
	- minimalに加え、WALアーカイビングに必要な情報を書き出す
  - hot_standby
	- archiveに加え、WALから実行中のトランザクション状態を再構築するのに必要な情報を書き出す。
	- レプリケーション環境で使用する。
	  - Streaming　Replication環境で、読み取り専用問い合わせを有効にする場合
  - logical
	- ロジカル・レプリケーション使用時の設定
- パラメータ変更
  - 再起動が必要(postmaster)
- 推奨値：archive
  - 理由：オンラインバックアップ（物理）運用にて必要な設定のため

#### archive_mode

	- 説明
  - チェックポイント済みWALセグメントをアーカイブするか否かの設定。
    - archive_modeを有効にするためには、wal_levelをarchiveまたはhot_standbyにする必要がある。
    - このパラメータを有効にした場合、archive_commandで設定されたコマンドで
    - WALセグメントのアーカイブ領域への退避を実施する
- 値
  - on：アーカイブ処理を実行する
  - off：アーカイブ処理を実行しない
- デフォルト値	off
- パラメータ変更	再起動が必要(postmaster)
- 推奨値	on
- 推奨理由	障害発生時のリカバリと、オンラインバックアップ（物理）による運用を可能とするため

#### archive_command

- 説明
  - 完了したWALファイルセグメントのアーカイブを実行するシェルコマンド
    - %p : 格納されるファイルの絶対パス
    - %f : ファイル名
- デフォルト値	空文字列（’’）
- パラメータ変更	オンラインで変更可能(sighup)
- 推奨値	‘cp -i %p /opt/PostgreSQL/9.4/archive/%f </dev/null’

#### archive_timeout

- 前回のセグメントファイル切り替えから archive_timeout 秒数経過した場合、新しいセグメントファイルに切り替わる。
  - archive_commandは、完了したWALセグメントに対してのみ呼び出されるため、サーバのWAL転送量が少ない場合は、アーカイブ格納領域への安全な記録までに長期の遅延が発生する。
  - archive_timeoutを設定することで、強制的に新しいWALセグメントに切り替える。
  - checkpoint_timeoutを大きくすると待機状態のシステム上のなくてもいいチェックポイントを削減できる。
  - 強制切り替えにより早期に作成されたアーカイブファイルも、完全に完了したアーカイブファイルと同じ大きさを持つつことに注意してください。
- 非常に小さなarchive_timeoutを使用すると、格納領域を膨張させてしまう。
- 通常１分程度のarchive_timeout設定が妥当
- もし高速にデータを他サーバにコピーしたいのであれば、アーカイブではなくストリーミングレプリケーションの選択を検討すべき

### checkpoint

#### checkpoint_timeout (integer)
- 自動的WALチェックポイント間の最大間隔を秒単位で指定
- 有効な範囲は、30秒から1日の間：デフォルトは5分（5min）
- このパラメータを増やすと、クラッシュリカバリで必要となる時間が増加

#### checkpoint_completion_target (floating point)
- チェックポイントの完了目標をチェックポイント間の総時間の割合として指定
- デフォルトは0.5

#### checkpoint_flush_after (integer)

- チェックポイント実行中にcheckpoint_flush_afterバイトより多く書く度に、OSが記憶装置に書き込むことを強制する。
- このことにより、カーネルのページキャッシュが持つダーティデータの量を一定量に制限し、チェックポイントの最後にfsyncが実行される際、あるいはOSがバックグラウンドでデータを大きな塊で書き出す際に性能の急激な低下を招く可能性を減らす。
- 多くの場合これによってトランザクションの遅延が大幅に少なくなりますが、あるケース、特にワークロードがshared_buffersよりも大きく、OSのページキャッシュよりも小さい時には性能が低下するかもしれません。 この設定が無効なプラットフォームがあります。 有効な設定値は、この機能が無効になる0から、2MBまでです。 デフォルト値は、Linuxでは256kBで、それ以外は0です。 (BLCKSZがデフォルト値でなければ、この設定のデフォルト値と最大値が変更されます。) このパラメータはpostgresql.confファイル、または、サーバのコマンドラインでのみで設定可能です。

#### checkpoint_warning (integer)

- チェックポイントセグメントファイルが溢れることが原因で起きるチェックポイントが、ここで指定した秒数よりも頻繁に発生する場合、サーバログにメッセージを書き出します （これは、max_wal_sizeを増やす必要があることを示唆しています）。 デフォルトは30秒（30s）です。 零の場合は警告を出しません。 checkpoint_timeoutがcheckpoint_warningより小さい場合警告を出しません。 このパラメータはpostgresql.confファイル、または、サーバのコマンドラインでのみ設定可能です。

#### max_wal_size (integer)

- pg_xlog配下に保持するWALファイルの合計サイズの上限を制御するパラメータ
- 指定したサイズ分の変更履歴がWALファイルに書かれた時にチェックポイントが行われる
- ソフトリミットのため、特別な状況下ではWALサイズはmax_wal_sizeを超える
  - 特別な状況：高負荷（archive_commandの失敗、wal_keep_segmentsが大きな値に設定されている、など）
- デフォルトは1GB（WALファイル64個分）
- このパラメータを大きくすると、クラッシュリカバリに必要な時間が長くなる。

#### min_wal_size (integer)

- pg_xlog配下に保持するWALファイルの合計サイズの下限
- デフォルト値は80MB（WALファイル5個分）
- この設定は、たとえば大きなバッチジョブを走らせる際に十分なWALのスペースが予約されていることを保証するために使用する。

# backup / recovery

- pg_dump　と　pg_dumpall
  - ファイルシステムレベルのバックアップを生成しないので、 継続的アーカイブ方式（オンラインバックアップ）の一部として使うことはできない。
  - このダンプは論理的なものであり、WALのやり直しで使うために必要な情報を含んでいない。

- オンラインバックアップ

- ベースバックアップ
  - ベースバックアップを取得する方法
	- pg_basebackup を実行する方法（最も簡単な方法）
	- 通常のファイルやTAR形式のファイルとしてのベースバックアップ取得
  - バックアップ履歴ファイル
    - ベースバックアップの過程で即座にWALアーカイブ領域に作成される。
	- ファイル名：ファイルシステムのバックアップに最初に必要とされるWALセグメントの名前が付けられる。
	  - 最初のWALファイルが 0000000100001234000055CDである場合、バックアップ履歴ファイルは0000000100001234000055CD.007C9330.backupというように名付けられる。
	  - ファイル名の2番目のパートはWALファイルの厳密な位置を示す。
	- バックアップ中に使用されたWALセグメントファイルより数値の小さなWALアーカイブは、取得したベースバックアップを利用したリカバリには必要無いため削除可能（しかし、数世代のバックアップセットを保持するためには保存を考慮すべき）

- タイムライン
  - アーカイブ復旧が完了したとき、復旧後に生成されたWAL記録を識別するための新しいタイムラインが生成される。
  - タイムラインID番号はWALセグメントファイル名の一部(前半部分の数字）
  - タイムラインを使用して、以前捨てたタイムライン分岐における状態を含む、過去の任意の状態に復旧させることができます。
  - history file(タイムライン履歴ファイル)
    - 新しいタイムラインが生成される度に、PostgreSQLは、どのタイムラインがいつどこから分岐したかを示すファイルを作成します。
    - この履歴ファイルは、複数のタイムラインを含むアーカイブ場所から復旧する時にシステムが正しいWALセグメントファイルを選択できるようにするために必要
    - WALアーカイブ領域にアーカイブされる。
  - 復旧処理のデフォルトは、ベースバックアップが取得された時点のタイムラインと同一のタイムラインに沿った復旧
    - 別の子タイムラインに沿って復旧させたい（つまり、復旧試行以降に生成されたある状態に戻りたい）場合はrecovery.confで対象のタイムラインIDを指定しなければなりません。
    - ベースバックアップより前に分岐したタイムラインに沿って復旧することはできません。

## recovery

- recovery.conf

  ```
  restore_command = 'cp /data/archive/%f "%p"'
  ```

  ```
  recovery_target_timeline='latest'
  # or
  recovery_target_timeline='n' # n is ~archive/00000<n>.history
  ```
