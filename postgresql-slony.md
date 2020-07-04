## slony

- URL
  - http://slony.info/
  - https://www.sraoss.co.jp/technology/postgresql/3rdparty/slony-I.php
  - https://powergres.sraoss.co.jp/s/ja/tech/plus/experience/vol10/largescale_database1.php

#### concepts

- cluster
  - PostgreSQLデータベースインスタンスの集合の名前
  - レプリケーションはこれらデータベースの間で行われる。
  - クラスタ名は、slonikスクリプトの以下の命令文で指定する。

    `cluster name = something;`

  - If the Cluster name is something, then Slony-I will create, in each database instance in the cluster, the namespace/schema_something.

- node
  - Nodeは、レプリケーションを構成するPostgreSQLデータベースのこと。
  - Nodeは、Slonikスクリプトの中の以下の命令で定義される。
    `NODE 1 ADMIN CONNINFO = ’dbname=testdb host=server1 user=slony’;`
  - SLONIK ADMIN CONNINFO(7)の情報は、最終的に、libpq関数のPQconnectdb()に渡されるデータベース接続情報を示す。
  - したがって、Slony-Iクラスタは以下を含む：
    - A cluster name
    - A set of Slony-I nodes, each of which has a namespace based on that cluster name

- replication set
  - テーブルと、Slony-Iのクラスタ内のノード間で複製されるシーケンスとして定義される。
  - 複数のreplication setを定義できる。これら複数のセットの間で、複製の"flow"（流れ）が同一である必要はない。

-  Origin, Providers and Subscribers
  - **origin node**
    - 各replication setにorigin nodeがある。
    - 複製されるテーブル内でアプリケーションがデータの修正を許可されている唯一のノードとなる。
    - "master provider"と呼ばれる。（データを供給する主な場所となる。）
  - **master provider**
    - origin nodeのこと
  - **provider**
    - master provider以外のprovider
    - カスケード先のproviderを示す。
  - **subscriber**
    - クラスタ内のorigin node以外の他のノードはデータを受信（subscribe）する。
    - origin nodeは、決してsubscriberにならない。
      - しかし、クラスタが再構成され、origin nodeが別のノードに明示的にシフトされた場合は別。
    - Slony-Iはカスケードされたサブスクリプションの概念をサポートしています。 その場合、susscriberは、カスケード先のレプリケーション・セットのクラスタ内の他のノードに対する「プロバイダ」としても動作する。

-  slon Daemon
  - クラスタ内の各ノードには、複製の動作を管理するslon(1)プロセスがある。
  - slon(1)は、C言語で作成されたプログラムで、以下2つの主なイベントがある。
    - Configuration events
      - slonik(1)スクリプトが実行したときに発生し、クラスタの設定に更新内容を送信する。
    - SYNC events
      - 複製されるテーブルの更新は、SYNCイベントとしてまとめられれる。
      - これらトランザクションのグループは、subscriber nodeに適用される。

- slonik Confguration Processor
  - slonik(1)コマンドプロセッサは、Slony-Iクラスタの設定の更新イベントを送信するための言語で記述されたスクリプトを実行する。
  - スクリプトには、ノードの削除、通信パスの変更、subscriptionの追加や削除などが記述される。


-  Slony-I Path Communications
  - Slony-IはPostgreSQL DSNを3つのコンテキストで使用して、データベースへのアクセスを確立する。

  - SLONIK ADMIN CONNINFO(7)
    - slonikスクリプトが各ノードに接続する方法を制御する。
    - これらの接続は、管理サーバからSlony-Iクラスタ内のすべてのノードへ行くための接続。
    - slonik（1）を実行する中央の場所からネットワーク内のすべてのノードに接続することが重要です。
    - これらの接続は、クラスタの管理を制御するために必要な少数のSQL要求を提出するためにのみ使用されます。
    - これらの通信パスは短時間しか使用されないため、SSHトンネリングを使用して一時的な接続を「ハックする」ことはかなり合理的です。
      - Since these communications paths are only used brie?y, it may be quite reasonable to “hack together” temporary connections using SSH tunnelling.
  - The slon(1) DSN parameter.
    - 各slon（1）に渡されるDSNパラメータは、slon（1）プロセスから管理するデータベースにどのネットワークパスを使用する必要があるかを示します。

  - SLONIK STORE PATH(7)
    - slon（1）デーモンがリモートノードと通信する方法を制御します。
    - これらのパスはsl_pathに格納されます。
    - 強制的に、各サブスクライバノードとそのプロバイダの間にパスを持つ必要があります。
      - 他のパスはオプションで、sl_listenのリッスンパス以外には使用されません。その特定の経路を使用する必要があります。

- PostgreSQL DSN (Data Source Name)
  -  PostgreSQL データベースに接続するための情報。
  - 構成要素は以下
    - DSN接頭辞: DSN 接頭辞は pgsql: です。
    - host
    - port
    - dbname:データベース名
    - user
    - password

- データベース認証の制御（パスワード）(Passwords)
  - slonとslonikプログラムは、PostgreSQLのクライアントとしてPostgreSQLに接続する。
  - PostgreSQLの認証は、通常のpg_hba.confファイルによるlibpq認証オプションで制御される。
  - 以下２つの方法で、必要になるパスワードを設定可能
    - SLONIK STORE PATH(7)命令
      - パスワードはデータベースの中に平文として保存する方法
    - .pgpassファイル
      - 同ファイルに、slonが動作している各々のノードの全経路についてのパスワードを設定する。

- How to remove replication for a node

  - You will want to remove the various Slony-I components connected to the database(s). We will just consider, for now, doing this to one node. If you have multiple nodes, you will have to repeat this as many times as necessary.
  - Components to be Removed:
    - Log Triggers / Update Denial Triggers
    - The “cluster” schema containing Slony-I tables indicating the state of the node as well as various stored functions
    - slon(1) process that manages the node
    - Optionally, the SQL and pl/pgsql scripts and Slony-I binaries that are part of the PostgreSQL build. (Of course, this would make it challenging to restart replication; it is unlikely that you truly need to do this...)
  - The second approach involves using psql to alter the table directly on each database in the cluster.
    1. Stop any application updates to the table you are changing(ie have on outage)
    1. Connect to each database in the cluster (in turn) and make the required changes to the table
  - **Warning**
    - The psql approach is only safe with Slony-I 2.0 or greater
  - Things are not fundamentally different whether you are adding a brand new, fresh node, or if you had previously dropped a node and are recreating it. In either case, you are adding a node to replication.

- How To Remove Replication For a Node
  - slonyをノードから削除するには以下２つの処理が必要
    - 問題のノードからslony schema（テーブル、関数、およびトリガー）を削除
    - 削除されたノードがもはや存在しないことを残りのノードに伝える
  - SLONIK DROP NODE(7)は、上記の両方を実施します。
  - SLONIK UNINSTALL NODE(7)コマンドは、ノードからslonyのスキーマだけを削除します。
  - 補足
    - SLONIK FAILOVER(7)コマンドを使用して別ノードに切り替えた障害が発生したノードでは、SLONIK UNINSTALL NODE(7)コマンドを使用してトリガーとスキームと関数を削除する必要があります。
  - **Warning**
    - Removing Slony-I from a replica in versions before 2.0 is more complicated. If this applies to you then you should consult the Slony-I documentation for the version of Slony-I you are using.

#### slonik commands

- SLONIK INIT CLUSTER
  - Slony-I clusterの初期化
  - Synopsis
    - INIT CLUSTER [ID = integer] [COMMENT = ’string’]
  - Description
	- 新しいSlony-Iレプリケーションクラスタで最初のノードを初期化する。
	- 初期化プロセスでは、以下の関数を使用して、クラスタ名空間の作成、すべての基本テーブル、関数、プロシージャのロード、ノードの初期化が行われる。
	  - schemadocinitializelocalnode(p_comment integer, p_local_node_id text)
	  - schemadocenablenode(p_no_id integer)
  - ID：ノードIDを指定
  - COMMENT =’comment text’
    - sl_node表のノード項目に追加された説明テキスト。
  - このプロセスを実行するには、Slony-IシステムのSQLスクリプトをDBAワークステーション（slonikユーティリティを現在実行しているコンピュータ）にインストールする必要があります。
  - 一方、ノードデータベースが実行されているシステムでは、 PostgreSQLライブラリディレクトリにSlony-Iシステムをインストールする必要がある。
  - また、プロシージャ言語PL / pgSQLはすでにターゲットデータベースにインストールされているものとします。

-  SLONIK CREATE SET
  - Create Slony-I replication set
  - Synopsis
    - CREATE SET (options);
  - Description
    - Slony-Iレプリケーションシステムでは、レプリケートされたテーブルがセットで構成されています。
    - 一般的な経験則として、セットには関係がある1つのアプリケーションのすべてのテーブルが含まれている
      - うまく設計されたアプリケーションでは、これは1つのスキーマのすべてのテーブルに等しい
    - 1つのノードが他のノードから複製するためにサブスクライブすることができる最小単位は1組です。
    - セットは常にoriginを持ちます。
    - Slony-Iでは、あるノードを「スレーブ」役割のレプリケーションデータを同時に受け取っている間に、ノードを「マスター」にすることができるため、この用語は、 簡単に誤解を招きやすいので、 "set origin"と "subscriber"に置き換えてください。
  - ID =ival
    - 作成するレプリケーションセットのID
  - ORIGIN =ival
    - レプリケーションセットのoriginノード
  - COMMENT =’string’
    - A descriptive text added to the set entry.

-  SLONIK SET ADD TABLE
  - Add a table to a Slony-I replication set
  - Synopsis
    - SET ADD TABLE (options);
  - Description
    - 既存のテーブルを、レプリケーションセットに追加する。
    - このレプリケーションセットは、現在、SLONIK MERGE SET(7)コマンドでサポートされる他のノードからサブスクライブされない。

    - SET ID =ival
      - テーブルを追加するレプリケーションセットID
    - ORIGIN =ival
      - レプリケーションセットのoriginノード(Optional)
    - ID =ival
      - テーブルのユニークID
      - テーブルの一意のID。
      - このIDは、すべてのセットで一意でなければなりません。同じIDを持つ同じクラスタ内に2つのテーブルを持つことはできません。
      - これらのIDは、複写システム内の個々の表を一意に識別するために使用されるだけでなく、このIDの数値は、たとえばSLONIK LOCK SET（7）コマンドでテーブルがロックされる順序も決定します。
    - FULLY QUALIFIED NAME =’string’
      - スキーマ名を含めたテーブル名。
	  - TABLESが指定されている場合は省略可能。
    - KEY ={ ’string’ | SERIAL } (Optional)
      - The index name that covers the unique and not null set of columns to be used as the row identi?er for replication purposes.
	  - Default is to use the table’s primary key. The index name is not fully quali?ed; you must omit the namespace.
    - TABLES =’string’
      - A POSIX regular expression that speci?es the list of tables that should be added.
	  - This regular expression is evaluated by PostgreSQL against the list of fully qualied table names on the set origin to?nd the tables that should be added.
	  - If TABLES is omitted then FULLY QUALIFIED NAME must be speci?ed.
        - Warning :....
      - COMMENT =’string’
        - A descriptive text added to the table entry.
		- ADD SEQUENCES=boolean A boolean value that indicates if any sequences attached to columns in this table should also be automatically added to the replication set. This defaults to false
    - This uses schemadocsetaddtable(p_tab_comment integer, p_tab_idxname integer, p_fqname text, p_tab_id name, p_set_id text).

- SLONIK STORE NODE
  - Initialize Slony-I node
  - Synopsis
    - STORE NODE (options);
  - Description
    - 新しいノードを初期化し、既存のクラスタの構成に追加します。
    - 初期化プロセスは、新しいノード（データベース自体がすでに存在していなければならない）にクラスタ名前空間を作成し、すべての基本表、関数、プロシージャをロードし、ノードを初期化することから成ります。 クラスタの残りの既存の構成は、「EVENT NODE」からコピーされます。
  - ID =ival
    - 新しいノードの一意で不変の数値ID番号。 IDは、ノード間イベント通信の基礎として使用されるため、不変であることに注意してください。
  - COMMENT =’description’
  - SPOOLNODE =boolean
    - Speci?es that the new node is a virtual spool node for ?le archiving of replication log. If true, slonik will not attempt to initialize a database with the replication schema.
    - Warning
      - Never use the SPOOLNODE value - no released version of Slony-I has ever behaved in the fashion described in the preceding fashion. Log shipping, as it ?nally emerged in 1.2.11, does not require initializing “spool nodes”.
  - EVENT NODE =ival
    - 新しいノードが追加されたことを既存のノードに示す構成イベントが作成されたノードの番号
	- このノードIDは、新しいノードIDではなく、クラスタ内に既に存在しているノードのIDを指定する。
  - This uses schemadocinitializelocalnode(p_comment integer, p_local_node_id text) and schemadocenablenode(p_no_id integer).

- SLONIK STORE PATH (options);
  - Description
    - ノードのレプリケーションデーモンが別のノードのデータベースに接続する方法を設定します。
    - Slony-Iが特別なバックボーンネットワークセグメントを使用する場合に、特別なIPアドレスまたはホスト名を設定できる場所（既存の設定を上書きする）
    - conninfo文字列には、複製するスーパーユーザーとしてデータベースに接続するためのすべての情報が含まれている必要があります。
    - 「サーバ」または「クライアント」という名前は、クラスタ構成内のノードの特定の役割とは何の関係もありません。
    - 単純に2ノードのセットアップでは、両方向へのパスを設定する必要があります。 すべてのノードから他のすべてのノードへのパス情報（フルクロスプロダクト）を設定することには何の害もありません。 サブスクリプションのためにリスン・エントリまたはデータのためにイベントまたは確認を実際に転送する必要がない限り、接続は確立されません。
  - SERVER =ival
    - Node ID of the database to connect to.
  - CLIENT =ival
    - Node ID of the replication daemon connecting.
  - CONNINFO =string
    - PQconnectdb() argument to establish the connection.
  - CONNRETRY =ival
    - Numberofsecondstowaitbeforeanotherattempttoconnectismadeincasetheserverisunavailable. Default is 10.
  - This uses schemadocstorepath(p_pa_connretry integer, p_pa_conninfo integer, p_pa_client text, p_pa_server integer).

- SLONIK SUBSCRIBE SET
  - SUBSCRIBE SET — Start replication of Slony-I set
  - Synopsis
	- SUBSCRIBE SET (options);
  - Description
	- This performs one of two actions:
	  - Initiates replication for a replication set Causes a node (subscriber) to start replicating a set of tables either from the origin or from another provider node, which must itself already be be an active, forwarding subscriber. The application tables contained in the set must already exist and should ideally be empty. The current version of Slony-I will not attempt to copy the schema of the set. The replication daemon will start copying the current content of the set from the given provider and then try to catch up with any update activity that happened during that copy process. After successful subscription, the tables are guarded on the subscriber, using triggers, against accidental updates by the application. If the tables on the subscriber are not empty, then the COPY SET event (which is part of the subscription process) may wind up doing more work than should be strictly necessary: – It attempts toTRUNCATEthe table, which will be efﬁcient. – Ifthatfails(aforeignkeyrelationshipmightpreventTRUNCATEfromworking),itusesDELETEtodeleteall“old”entries in the table – Those old entries clutter up the table until it is nextVACUUMed after the subscription process is complete – The indices for the table will contain entries for the old, deleted entries, which will slow the process of inserting new entries into the index.


- SLONIK DROP NODE
 - Remove the node from participating in the replication
 - This command removes the speci?ed node entirely from the replication systems con?guration. If the replication daemon is still running on that node (and processing events), it will attempt to uninstall the replication system and terminate itself.
  - ID =ival
    - Node ID of the node to remove. This may be represented either by a single node id or by a quoted comma separated list of nodes
  - EVENT NODE =ival
    - Node ID of the node to generate the event.

 - DROP NODE ( ID = 2, EVENT NODE = 1 );
 - DROP NODE (ID=’3,4,5’, EVENT NODE=1);

- SLONIK UNINSTALL NODE
  - Decommission Slony-I node
  - Restores all tables to the unlocked state, with all original user triggers, constraints and rules, eventually added Slony-I speci?c serial key columns dropped and the Slony-I schema dropped. The node becomes a standalone database. The data is left untouched.
    - ID =ival
      - Node ID of the node to uninstall.
  - The difference between UNINSTALL NODE and DROP NODE is that all UNINSTALL NODE does is to remove the Slony-I con?guration; it doesn’t drop the node’s con?guration from replication.
  - UNINSTALL NODE ( ID = 2 );

