## OpenLDAP


## RHEL7 OpenLDAP
<https://access.redhat.com/documentation/ja-JP/Red_Hat_Enterprise_Linux/7/html/System_Administrators_Guide/ch-Directory_Servers.html>

### OpenLDAP

- LDAP (Lightweight Directory Access Protocol)
  - ネットワーク上で中央に保存されている情報へアクセスするために使用するオープンプロトコルのセット
  - ディレクトリー共有のために X.500 標準をベースとしている
  - それほど複雑ではなくリソース集約型
  - このため LDAP は 「X.500 Lite」 と呼ばれることがある。

  - ディレクトリーを使用して階層式で情報を組織化する。
  - ディレクトリーには、名前やアドレス、電話番号などの各種情報を保存することができる。
  - ネットワーク情報サービス (NIS) と似た方法で使用することもできる。
  - 誰でも LDAP 対応ネットワーク上にあるマシンから自分のアカウントにアクセスできます。
  - LDAP の一般的な使用目的
    - 中央で管理されるユーザーとグループ、ユーザー認証、システム設定
	- 仮想の電話帳としても使用できる
	- ユーザーは他のユーザーの連絡先情報に容易にアクセスできます。
	- 世界各地にある他の LDAP サーバーにユーザーを照会して、グローバルレポジトリーに保存されているアドホックな情報を提供することが可能

- OpenLDAP 2.4
  - LDAPv2 および LDAPv3 プロトコルのオープンソース実装

### LDAP概要

- LDAP は、クライアント/サーバーのアーキテクチャー
- ネットワークからアクセス可能な中央情報ディレクトリーを作成する、信頼できる手段を提供します。
- Transport Layer Security (TLS) 暗号化プロトコルを使用

##### 重要
- Red Hat Enterprise Linux 7 の OpenLDAP スイートは OpenSSL を使用しなくなりました。
- ネットワークセキュリティーサービス (NSS) の Mozilla 実装を使用します。
- OpenLDAP は現行の証明書、鍵、その他の TLS 設定による機能を継続します。
- Mozilla 証明書と鍵データベースを使用するための Mozilla NSS の設定方法については、『How do I use TLS/SSL with Mozilla NSS (Mozilla NSS で TLS/SSL を使用する方法)』 を参照

##### 重要


#### OpenLDAPのインストール


#### LDAP の用語

- エントリー
  - LDAP ディレクトリー内の単一ユニットです。各エントリーは一意の DN (Distinguished Name: 識別名) で識別されます。

- 属性
  - エントリーに直接関連付けられた情報です。
  - 属性には、単一値か順不同の空白で区切られた値の一覧のどちらかがあります。
  - 一部の属性はオプションですが、その他は必須です。
  - 必須の属性は objectClass 定義を使用して指定され、/etc/openldap/slapd.d/cn=config/cn=schema/ ディレクトリー内のスキーマファイル内にあります。
  - 属性とそれに対応する値のアサーションは、RDN (Relative Distinguished Name: 相対識別名) とも呼ばれます。
  - グローバルで一意の識別名とは異なり、相対識別名は エントリーに対してのみ一意なものです。

- LDIF
  - LDAP データ交換形式 (LDIF) は LDAP エントリーをプレーンテキスト形式で表示したものです。

    [id] dn: distinguished_name
    attribute_type: attribute_value…
    attribute_type: attribute_value…
    …

#### OpenLDAP サーバーユーティリティー

- slapacl
  - 属性の一覧へのアクセスをチェック

- slapadd
  - LDIF ファイルから LDAP ディレクトリーへエントリーを追加

- slapauth
  - 認証および承認のパーミッション用に ID の一覧をチェック

- slapcat
  - LDAP ディレクトリーからデフォルト形式でエントリーをプルして、LDIF ファイル内に保存

- slapdn
  - 利用可能なスキーマ構文をベースとした識別名 (DN) の一覧をチェック

- slapindex
  - 現在の内容を基にして slapd ディレクトリーのインデックスを再構築
  - 設定ファイル内のインデックスオプションを変更する時は常にこのユーティリティーを実行します。

- slappasswd
  - ldapmodify ユーティリティーでの利用もしくは、slapd 設定ファイル内で使用する暗号化されたユーザーパスワードを作成する。

- slapschema
  - データベースの整合性を対応するスキーマとチェックする。

- slaptest
  - LDAP サーバーの設定をチェックする。

#### OpenLDAP クライアントユーティリティー


command | description
-----| -----
ldapadd     |	ファイルまたは標準入力からのエントリーを LDAP ディレクトリーに追加する。（ldapmodify -a へのシンボリックリンク）
ldapcompare	|任意の属性と LDAP ディレクトリーのエントリーを比較する。
ldapdelete	|LDAP ディレクトリーからエントリーを削除する。
ldapexop	   |LDAP の拡張操作を実行する。
ldapmodify	|LDAP ディレクトリー内のエントリーをファイルまたは標準入力から修正する。
ldapmodrdn	|LDAP ディレクトリーエントリーの RDN 値を修正する。
ldappasswd	|LDAP ユーザー用のパスワードを設定し変更する。
ldapsearch	|LDAP ディレクトリーエントリーを検索する。
ldapurl	|LDAP URL を生成または分解する。
ldapwhoami	|LDAP サーバー上で whoami 動作を実行する。


#### OpenLDAP サーバーの設定

- /etc/openldap/ldap.conf
  - OpenLDAP ライブラリーを使用するクライアントアプリケーション用の設定ファイル
  - これには ldapadd、ldapsearch、Evolution などが含まれます。

- /etc/openldap/slapd.d/
  - slapd 設定を含む階層ディレクトリー構造
  - これらのエントリーを編集するための推奨される方法は 「OpenLDAP サーバーユーティリティーの概要」 にあるように**サーバーユーティリティーを使用**する方法

- **注意**
  - OpenLDAP は設定の /etc/openldap/slapd.conf ファイルからの読み取りを実行しなくなった。
  - 代わりに、/etc/openldap/slapd.d/ ディレクトリーに存在する設定データベースを使用する。
- **重要**
  - LDIF ファイル内でエラーが発生すると slapd サービスが起動できなくなることがあります。このため、/etc/openldap/slapd.d/ 内の LDIF ファイルを直接編集しないことを強くお勧めします。

#### グローバル設定の変更

LDAP サーバーのグローバル設定オプションは、/etc/openldap/slapd.d/cn=config.ldif ファイル内に格納されています。
一般的に使用される指示文は、以下のとおりです。

    olcAllows

olcAllows 指示文を使用すると、有効にする機能を指定できます。以下の形式を取ります。

    olcAllows: feature…


プション	詳細


optin | 詳細
----|-----
bind_v2	| LDAP バージョン 2 のバインド要求の受け入れを有効にします。
bind_anon_cred	| 識別名(DN) が空欄である場合の匿名バインドを有効にします。
bind_anon_dn	| 識別名(DN) が空欄で ない 場合の匿名バインドを有効にします。
update_anon	| 匿名による更新操作の処理を有効にします。
proxy_authz_anon	| 匿名によるプロキシー認証制御の処理を有効にします。
⁠
例13.1 olcAllows 指示文の使用

    olcAllows: bind_v2 update_anon

##### olcConnMaxPending
olcConnMaxPending 指示文を使用すると、匿名セッションに対する保留中の要求の最大数を指定できます。以下の形式を取ります。

    olcConnMaxPending: number

##### olc****
その他多数

#### データベース特有の設定の変更

- デフォルトでは、OpenLDAP サーバーは Berkeley DB (BDB) をデータベースバックエンドとして使用します。
- このデータベース用の設定は /etc/openldap/slapd.d/cn=config/olcDatabase={1}bdb.ldif ファイルに格納されています。
- データベース特有の設定でよく使用される指示文は、以下のとおりです。

###### olcReadOnly
- 指示文を使用すると、データベースを読み取り専用モードで使用できます。    
- これは、TRUE (読み取り専用モードを有効) または FALSE (データベースの修正を有効) を使用します。
- デフォルトのオプションは FALSE です。
⁠

    olcReadOnly: TRUE

###### olcRootDN
- アクセス制御により制限されないユーザーまたは LDAP ディレクトリの動作用に設定される管理制限パラメーターを指定できます。
- 以下の形式を取ります。

    olcRootDN: distinguished_name

- これは、識別名 (DN) を使用します。
- デフォルトのオプションは以下

    cn=Manager,dn=my-domain,dc=com





#### インストールされているドキュメント

- 以下のドキュメントは、openldap-servers パッケージでインストールされます。

  - /usr/share/doc/openldap-servers-version/guide.html
    - 『OpenLDAP Software Administrator's Guide』 のコピー。

  - /usr/share/doc/openldap-servers-version/README.schema
    - インストールされているスキーマファイルが含まれる README ファイル。





CentOS7にOpenLDAPサーバをインストールし設定します。まず必要なパッケージをyumからインストールします。

    # yum install openldap openldap-servers openldap-clients
    # rpm -qa | grep openldap
    openldap-2.4.39-3.el7.x86_64
    openldap-servers-2.4.39-3.el7.x86_64
    openldap-clients-2.4.39-3.el7.x86_64


サービスの起動

    # systemctl start slapd


動作確認

    $ /usr/bin/ldapsearch -x -h localhost -b dc=my-domain,dc=com -LLL

設定ファイルのチェック

    # slaptest -u -d 64 -f /etc/openldap/ldap.conf

##### 警告
データの整合性を維持するために、slapadd、slapcat、slapindex を使用する前に slapd サービスを停止します。シェルプロンプトで以下を入力します。

    # systemctl stop slapd.service





- OpenLDAP2.3からConfiguration Backendという機能がサポートされる。
- テキストファイルに記載して設定を実施するのではなく、LDIFファイルを適用して設定をデータベースに格納し管理する機能
- リモートから設定変更が可能であったり変更後の再起動が不要などのメリットがある.
- やリ方が結構煩わしい

Configuration Backendを前提で設定する場合はCentOS6の以下参照
<http://www.unix-power.net/linux/openldap.html>

### LDAPの概要

LDAP をイメージしやすくするためにおなじみのファイルシステムと対比してみます。

- Linuxのファイルシステム
  - /を頂点とする階層構造を持ちます。
  - /にはetcやhomeといった子ディレクトリが存在し各ディレクトリにはファイルや子ディレクトリが存在します。
  - ディレクトリやファイルにはそれを識別するためのi-node番号という情報が割り当てられています。
  - 例えば/には直下にあるファイルやディレクトリの名前と i-node番号の対応情報が格納サれています。
  - 同様に/etcにはファイル「/etc/hosts」など/etcディレクトリ直下にあるファイルやディ レクトリの名前とi-node番号の対応情報が格納されています。
  - ディレクトリにはi-node番号の他にもファイルのサイズ、パーミッション、タイムスタンプといった情報も格納されています。
  - つまりファイルシステムにおけるディレクトリは子ファイルや子ディレクトリの名前とi-node番号の対応情報やサイズ、パーミッションといった情報を提供するディレクトリサービスと言えます。
- LDAPのディレクトリ構造
  - ファイルシステムにおけるディレクトリが直下のファイルやディレクトリの情報をもつようにLDAPディレクトリのエントリは「属性」を呼ばれる情報を持ちます。
    - LDAPディレクトリでは属性情報の種類を利用者が定義できます。
    - このようなLDAPの特徴をいかすと普段はパスワードファイル「/etc/passwd」に格納されているLinuxのシステムのアカウント情報やメーラのアドレス帳で管理されている住所録データを LDAPサーバに格納できます。

- ディレクトリのエントリには相対識別名 ( RDN : Relative Distnguish Name ) と呼ばれる名前が付けられます。
  - 相対識別名は「属性＝属性値」という形式で記述され、例えばunix-powerドメインを表すエントリの相対識別名は「dc=unix-power」となります。
  - またエントリを一意に表すために識別名 ( DN : Distinguish Name ) と呼ばれる名前があります。
  - これは木構造の頂点に位置するエントリと当該エントリ間に存在する全エントリの相対識別名(RDN)を連結して記述 されます。
  - 例えば下記に示すUsersエントリの識別名(DN)は「ou=Users,dc=unix-power,dc=net」となります。

#### 属性の種類

- LDAP ディレクトリの各エントリには属性情報が格納されています。
- 属性情報は「属性名」と「属性値」の対で構成されます。
- 属性の種類は様々で例えばcn属性 (Common Name)の値は人物の名前などです。また、dc属性(Domain Component)の値はDNSのドメイン名などです。
- このうちObjectClass属性は少し特殊
  - この属性で指定される値はスキーマと呼ばれ、全てのエントリで指定する必要があります。
  - スキーマでは当該エントリに格納されなければならない属性や格納できる属性などが定義されています。
  - 例えば「organizationUnit」スキーマが指定されたエントリにはou属性を必ず指定する必要があります。また「dcObject」スキーマを指定したエントリにはdc属性が必須です。
  - 逆 に言えばLDAPディレクトリにエントリを追加する際はまず電話番号や氏名といった自分の登録したい情報の種類を整理しそれらを格納するための属性(cnやdc)を決めます。それからその属性を格納するためのスキーマを探すことになります。

- OpenLDAPでは他のサービスプログラムのようにファイルで設定を管理するのではなくディレクトリサービスを使って管理することができるようになっています。
- この管理のためのディレクトリを設定用ディレクトリ ( Config Diretory )と呼びます。
- それに対してLinuxユーザアカウントをのデータベースなどを保管する場所をデータ用ディレクトリと呼びます。
