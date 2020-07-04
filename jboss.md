## URL
 
- EAP 7 : admin manual
  - https://access.redhat.com/documentation/ja-jp/red_hat_jboss_enterprise_application_platform/7.0/html/configuration_guide/

## configuration

##### deployment timeout

- error

  ```
  ERROR [org.jboss.as.controller.management-operation] (Controller Boot Thread) WFLYCTL0348: Timeout after [300] seconds waiting for service container stability. Operation will roll back. Step that first updated the service container was 'add' at address '[
    ("core-service" => "management"),
    ("management-interface" => "http-interface")
  ]'

  ERROR [org.jboss.as.controller.management-operation] (Controller Boot Thread) WFLYCTL0348: サービスコンテナが安定するまで [300] 秒待機したあとにタイムアウト。操作はロールバックされます。
  ```
  - fix:
    - To increase the timeout, there are 2 properties you might be interested in adjusting - timeout for the management module (read from a system property), and timeout for the deployment scanner (only used in standalone mode). The following CLI snippet takes care of both, using the value of 15 minutes (=900 seconds) for the timeouts:

    ```
    /system-property=jboss.as.management.blocking.timeout:add(value=900)

    /subsystem=deployment-scanner/scanner=default:write-attribute(name=deployment-timeout,value=900)
    ```

## hello world

### Configure Maven to Build and Deploy the Quickstarts

- https://github.com/jboss-developer/jboss-developer-shared-resources/blob/master/guides/CONFIGURE_MAVEN_JBOSS_EAP7.md

- download Maven (> 3.2.0)

  - [Download Maven](http://maven.apache.org/download.html)
  
- install Maven

## config file

#### snapshot

- スナップショットの作成
  - 管理 CLI を使用して、現在の設定のスナップショットを作成します。
    ```
    :take-snapshot
    ```

- スナップショットのリスト
  - 管理 CLI を使用して、作成したすべてのスナップショットをリストします。
    ```
    :list-snapshots    
    ```

- スナップショットの削除
  - 管理 CLI を使用して、スナップショットを削除します。
    ```
    :delete-snapshot(name=20151022-133109702standalone.xml)
    ```

- スナップショットを用いたサーバーの起動
  - スナップショットまたは自動保存された設定を使用してサーバーを起動できます。

    - EAP_HOME/standalone/configuration/standalone_xml_history ディレクトリーへ移動し、ロードするスナップショットまたは保存された設定ファイルを確認します。

    - サーバーを起動し、選択した設定ファイルを示します。設定ディレクトリー EAP_HOME/standalone/configuration/ からの相対パスを渡します。

    ```
    $ EAP_HOME/bin/standalone.sh --server-config=standalone_xml_history/snapshot/20151022-133109702standalone.xml    
    ```

#### 設定変更の確認
- 設定変更の追跡を有効

  ```
  /core-service=management/service=configuration-changes:add(max-history=10)
  ```

- 設定変更のリスト表示

  ```
  /core-service=management/service=configuration-changes:list-changes
  ```

#### システムプロパティー

- 起動スクリプトにシステムプロパティーを渡す
  - JBoss EAP 起動スクリプトにシステムプロパティーを渡すには -D 引数を使用します。以下に例を示します。
    ```
    $ EAP_HOME/bin/standalone.sh -Djboss.bind.address=192.168.1.2
    ```

- 管理 CLI を使用したシステムプロパティーの設定
  ```
  /system-property=PROPERTY_NAME:add(value=PROPERTY_VALUE)

  /system-property=jboss.bind.address:add(value=192.168.1.2)
  ```

- 管理コンソールを使用したシステムプロパティーの設定

  - スタンドアロン JBoss EAP サーバーでは、管理コンソールの Configuration タブでシステムプロパティーを設定できます。System Properties を選択し、View ボタンをクリックします。

- JAVA_OPTS を使用したシステムプロパティーの設定

  - スタンドアロンサーバーの場合、このファイルは EAP_HOME/bin/standalone.conf
  - 管理対象ドメインの場合は、EAP_HOME/bin/domain.conf

  ```
  # Set the bind address
  JAVA_OPTS="$JAVA_OPTS -Djboss.bind.address=192.168.1.2"
  ```
## NAMING サブシステムの設定

- NAMING サブシステム
  - NAMING サブシステムは JBoss EAP のJNDI 実装を提供する。
  - このサブシステムを設定して以下が可能
    - グローバル JNDI 名前空間のエントリーのバインド
    - リモート JNDI のアクティブまたは非アクティブ化

- グローバルバインディングの設定

  - エントリーを以下のJNDI名前空間へバインド可能
    - java:global (推奨)
    - java:jboss
    - java グローバル
  - グローバルバインディングは naming サブシステムの <bindings> 要素で設定される。
  - 以下の 4 種類のバインディングがサポートされる。
    - シンプルバインディング
    - オブジェクトファクトリーバインディング
    - 外部コンテキストバインディング
    - バインディングルックアップエイリアス

  - 外部コンテンツのバインド
    - LDAP コンテキストなどの外部 JNDI コンテキストのフェデレーションは、external-context XML 設定要素を使用して実行されます。

      - name 属性は必須で、エントリーのターゲット JNDI 名を指定します。
      - class 属性は必須で、フェデレートされたコンテキストの作成に使用される Java 初期ネーミングコンテキストタイプを示します。このようなタイプには、単一の環境マップ引数を持つコンストラクターが必要なことに注意してください。
      - 任意の module 属性は、外部 JNDI コンテキストが必要とするすべてのクラスをロードできる JBoss Module ID を指定します。
      - オプションの cache 属性は外部コンテキストインスタンスをキャッシュする必要があるかどうかを示し、デフォルトは false になります。
      - 任意の environment 子要素は、外部コンテキストを検索するために必要なカスタム環境を提供するために使用されます。
    - jboss-cliコマンドの例

      ```
      /subsystem=naming/binding=java\:global\/federation\/ldap\/example:add(binding-type=external-context, cache=true, class=javax.naming.directory.InitialDirContext, environment=[java.naming.factory.initial=com.sun.jndi.ldap.LdapCtxFactory, java.naming.provider.url=ldap\:\/\/ldap.example.com\:389, java.naming.security.authentication=simple, java.naming.security.principal=uid\=admin\,ou\=system, java.naming.security.credentials= secret])
      ```
    - バインディングの削除
      ```
      /subsystem=naming/binding=java\:global\/federation\/ldap\/example:remove
      ```

    - 結果の XML 設定

      ```
      <subsystem xmlns="urn:jboss:domain:naming:2.0">
          <bindings>
              <external-context name="java:global/federation/ldap/example" class="javax.naming.directory.InitialDirContext" cache="true">
                  <environment>
                      <property name="java.naming.factory.initial" value="com.sun.jndi.ldap.LdapCtxFactory" />
                      <property name="java.naming.provider.url" value="ldap://ldap.example.com:389" />
                      <property name="java.naming.security.authentication" value="simple" />
                      <property name="java.naming.security.principal" value="uid=admin,ou=system" />
                      <property name="java.naming.security.credentials" value="secret" />
                  </environment>
              </external-context>
          </bindings>
      </subsystem>
      ```

## jboss-cli.sh

### basic usage
```
$ jboss-cli -connect
$ jboss-cli -c --controller=IP_ADDRESS:port
```

### sample operation:
```
	connect

	cd subsystem
	ls
	cd deployment-scanner
	cd scanner
	cd default
	:read-resource
	{
	    "outcome" => "success",
		    "result" => {
		        "auto-deploy-exploded" => false,
		        "auto-deploy-xml" => true,
		        "auto-deploy-zipped" => false,
		        "deployment-timeout" => 600,
		        "path" => "deployments",
		        "relative-to" => "jboss.server.base.dir",
		        "scan-enabled" => true,
		        "scan-interval" => 5000
	    }
	}

	cd /
	:shutdown
```
### Recipes

- URL: https://docs.jboss.org/author/display/AS71/CLI+Recipes

```
$ ./bin/jboss-cli.sh --connect --controller=IP_ADDRESS
$ ./bin/jboss-cli.sh -c --controller=IP_ADDRESS
```

##### Runtime

##### Runtime

- Get all configuration and runtime details from CLI
  ```
  ./bin/jboss-cli.sh -c command=":read-resource(include-runtime=true, recursive=true, recursive-depth=10)"
  ```


##### Properties

###### Adding, reading and removing system property using CLI

```
[standalone@IP_ADDRESS:9999 /] /system-property=foo:add(value=bar)
[standalone@IP_ADDRESS:9999 /] /system-property=foo:read-resource
[standalone@IP_ADDRESS:9999 /] /system-property=foo:remove

[domain@IP_ADDRESS:9999 /] /system-property=foo:add(value=bar)
[domain@IP_ADDRESS:9999 /] /system-property=foo:read-resource
[domain@IP_ADDRESS:9999 /] /system-property=foo:remove

Host and its server instances
[domain@IP_ADDRESS:9999 /] /host=master/system-property=foo:add(value=bar)
[domain@IP_ADDRESS:9999 /] /host=master/system-property=foo:read-resource
[domain@IP_ADDRESS:9999 /] /host=master/system-property=foo:remove

Just one server instance
[domain@IP_ADDRESS:9999 /] /host=master/server-config=server-one/system-property=foo:add(value=bar)
[domain@IP_ADDRESS:9999 /] /host=master/server-config=server-one/system-property=foo:read-resource
[domain@IP_ADDRESS:9999 /] /host=master/server-config=server-one/system-property=foo:remove
```

###### Overview of all system properties

```
[standalone@IP_ADDRESS:9999 /] /core-service=platform-mbean/type=runtime:read-attribute(name=system-properties)

[domain@IP_ADDRESS:9999 /] /host=master/core-service=platform-mbean/type=runtime:read-attribute(name=system-properties)
[domain@IP_ADDRESS:9999 /] /host=master/server=server-one/core-service=platform-mbean/type=runtime:read-attribute(name=system-properties)
```

###### List Subsystems

```
[standalone@localhost:9999 /] /:read-children-names(child-type=subsystem)
```

###### List description of available attributes and childs

```
/subsystem=datasources/data-source=ExampleDS:read-resource-description
```

### deployment scanner

  設定ファイル：standalone/configuration/standalone.xml
    default config :
        <subsystem xmlns="urn:jboss:domain:deployment-scanner:1.1">
            <deployment-scanner path="deployments" relative-to="jboss.server.base.dir" scan-interval="5000"/>
        </subsystem>

  手動デプロイの設定：
	auto-deploy-zipped="false"を追加する。

            <deployment-scanner path="deployments" relative-to="jboss.server.base.dir" scan-interval="5000" auto-deploy-zipped="false"/>
 auto-deploy-zipped="false"/>

　動作：
	起動時に配備されていたAPをデプロイする。
	停止後にdeploymentディレクトリから削除したAPは、再起動時に削除される。
	マーカーファイルでの操作は可能。

	もし、起動時だけデプロイしたいということであればintervalを0に設定する。
		scan-interval="0"

	注意：
		scan-enabled="false"にすると、起動時もdeploymentディレクトを
		チェックしなくなってしまう。


### jmx-console
 user/passwordの設定

  deploy/jmx-console.war/WEB-INF/jboss-web.xml
    <<<<< COMMENT OUT >>>>>
      <security-domain>java:/jaas/jmx-console</security-domain>

  deploy/jmx-console.war/WEB-INF/web.xml
    <<<<< COMMENT OUT >>>>>
      <security-constraint>
      </security-constraint>

  conf/login-config.xml

  conf/props/jmx-console-users.properties
    user_name=password

  conf/props/jmx-console-roles.properties
    user_name=JBossAdmin

hsqldb
  console:
    $ java -classpath .;../lib/hsqldb.jar org.hsqldb.util.DatabaseManager

### Tuning

!!!WEBコンテナ
!!META-INF/jboss-service.xml
@CLASSLOADER
!クラスローダー
-Java2ClassLoadingCompliance
--デフォルトはfalseなのでtrueに設定
--servlet 2.3のwebコンテナを最初に見るようなモデル
  ではなく標準のjava2の最初のclass loadingモデルを使
  用するように設定
-UseJBossWebLoader
--デフォルトはfalseなのでtrueに設定
--JBoss Loaderを使用するように設定。このローダは、
  tomcat特有のclass loaderを使用するのではなく
  class loaderとしてUnifiedClassLoaderを使用
@TOMCAT
!!server.xml
!HTTP / AJP Connector
-maxThreads
--Connectorがリクエストを処理するために作成するス
  レッドの最大数。同時に扱うことの出来るリクエストの
  最大数。デフォルトは200
-minSpareThreads
--Connectorが最初に作成されたときに作成されるスレッ
  ドの数。また、アイドルスレッドの最小値。
  maxThreadsより少なく設定する。デフォルト値は4

### HTTP Connector
-acceptCount
--全てのスレッドが使用中のときに、入ってくるリクエ
  ストをキューに保持することの出来る最大数。キューが
  一杯のときに入ってきたリクエストはrefuseされる。デ
  フォルト値は10

!!!Datasource Connection Pooling
!!deploy/jboss40-oracle-ds.xml
!min-pool-size
-コネクションプールの最小値
!max-pool-size
-コネクションプールの最大値

#### ADD
!!!Cluster
!!tc5-cluster-service.xml
!CacheMode
-キャッシュの変更を同期的に行なうか非同期的に行なうかを指定
--REPL_SYNC
--REPL_ASYNC
!IsolationLevel
-キャッシュをトランザクショナルに更新する場合の
Isolationレベルを設定。各値の意味は、データベース
におけるisolationレベルと同じ。デフォルトは、REPEATABLE_READ
--SERIALIZABLE
--REPEATABLE_READ
--READ_COMMITED
--READ_UNCOMMITED
!!WEB-INF/jboss-web.xml
!replication-trigger
-SET
--
-SET_AND_GET
--
-SET_AND_NON_PRIMITIVE_GET
--
-ACCESS
--
!replication-granularity
-session
--
-attribute
--

 - JGroupsの設定
    バッファの設定
 - JBoss Cacheの設定
    キューなど
!!Stateful Session Bean
!
   isModified()(？)関数の追加



### Cluster

HTTP session state replication
  JBossの設定
    allから以下のファイルをコピー
      - deploy/tc5-cluster-service.xml
      - lib/jgroups.jar
      - lib/jboss-cache.jar

 tc5-cluster-service.xmlの設定

  CacheMode
    REPL_SYNC
    REPL_ASYNC

    同期レプリケーション or 非同期レプリケーション

    非同期の場合は、Replication Queueの使用を勧める。

  LockAcquisitionTimeout
    ロックを獲得する時のタイムアウト(msec)

  UseReplQueue (true/fause)
    Replicatin Queueを使用するか/しないか

  ReplQueueInterval
    Replication Queueにあるデータを送信する間隔の指定(msec)

  ReplQueueMaxElements
    queueの最大値

  IsolationLevel
    SERIALIZABLE
    REPEATABLE_READ (*)
    READ_COMMITED
    READ_UNCOMMITED

    SERIALIZABLE
      トランザクションが完全に実行を完了するまで、
      対象となったオブジェクトを完全にロックし、ほ
      かのトランザクションからのデータの挿入や更新
      を不可能にする。

    REPEATABLE_READ (*)
      トランザクションにおいて対象となるすべてのテー
      ブルの対象データがトランザクション実行中に変更
      されないことを保証
      ただし、トランザクションの対象となるテーブル
      に新規にデータが追加されたり、対象ではないデー
      タが削除されたりすることは防止ししない。

    READ_COMMITED
      コミット済みのデータしか読み取らないモード
      ほかのトランザクションが対象となるオブジェク
      トを更新中の場合は、コミットされるまで待つ

      対象のテーブルのダーティリードを回避可能
      しかし、1つのトランザクション中で2回以上そのテー
      ブルを参照する場合、1回目に読み取るデータと2
      回目に読み取るデータが同じであることは保証さ
      れない

    READ_UNCOMMITED
      対象となるオブジェクトに対するトランザクショ
      ンが完了していなくても、現在の最新状態を読み
      取るモード

 アプリケーションでの設定
  jboss-web.xml
    replication-config
      replication-trigger
      replication-granularity

  replication-trigger
    SET
    SET_AND_GET
    SET_AND_NON_PRIMITIVE_GET(default)
    ACCESS
  replication-granularity
    セッションレプリケーションを行なう単位
      session ..... session全部
      attribute ... sessionの中にあるレプリケーション
                    対象のattributeだけ

## transaction

#### commit option

option A
  トランザクションの間、entityの値やステートをキャッシュに保持する。
  コンテナは、パーシステントに対して唯一のユーザという前提のオプション。
  コンテナは、絶対的に必要になったときにだけキャッシュとパーシステントの内容を同期させる。
  トランザクション中で実行されるかされないかに依存して動作が変わる。
  他のユーザがアクセスした場合は考慮していない。
  このため、他でデータが変更された場合には対応出来ない。
  インスタンスは、キャッシュされ、トランザクション中使用される。

option B(default)
  トランザクション中、entiryのコンテキスト情報を保持する。
  コンテナは、トランザクションの最初にキャッシュの内容とパーシステントの内容を同期させる。
  トランザクション中で使用する場合は、キャッシュすることにあまり意味がありません。
  トランザクション外で使用する場合は、キャッシュされた値を使用する。
  インスタンスは、キャッシュされ、トランザクション中使用される。
  しかし、トランザクションの最後にdirtyフラグを立てる。
  新しいトランザクションが実行される時は、ejbLoadが必ずコールされる。

option C
  コンテナはbeanインスタンスをキャッシュしない。
  トランザクションが開始する時に、ステートは同期される。
  トランザクション外で実行する場合も、同期する。
  しかし、ejbLoadは同一トランザクション内で実行される。
  トランザクションが終了するまで、全てのentiryコンテキスト情報を破棄する。
  インスタンスはdirtyフラグを立てられ、キャッシュから解放される。
  トランザクションの最後に、passivationのためにdirtyフラグが立てられる。

option D
  JBoss特有のオプション。
  基本的には、option A。
  データは一定の時間だけ有効となるようなタイマが設定される。
  ステートは定期的に同期がとられる。(default 30 sec : optiond-refresh-rate )

### EntityCache
  jboss.xmlの<instance-cache>で指定したInstanceCacheが使用される。

  public interface InstanceCache
   extends ContainerPlugin

  public interface EntityCache
   extends InstanceCache

  public abstract class AbstractInstanceCache
   implements InstanceCache, XmlLoadable, Monitorable, MetricsConstants,
      AbstractInstanceCacheMBean

  public class EntityInstanceCache
   extends AbstractInstanceCache
   implements org.jboss.ejb.EntityCache, EntityInstanceCacheMBean


  public class EntityContainer
   extends Container
   implements EJBProxyFactoryContainer, InstancePoolContainer,
      EntityContainerMBean

   public void setInstanceCache(InstanceCache ic)
   {
      if (ic == null)
         throw new IllegalArgumentException("Null cache");

      this.instanceCache = ic; <<<<<<<<<
      ic.setContainer(this);
   }

  public class EjbModule
   extends ServiceMBeanSupport
   implements EjbModuleMBean

   private EntityContainer createEntityContainer(BeanMetaData bean,
                                                 ClassLoader cl,
                                                 ClassLoader localCl)
      throws Exception
   {
      container.setInstanceCache(createInstanceCache(conf, cl));

   private static InstanceCache createInstanceCache(..)
      throws Exception
   {
      try
      {
         ic = (InstanceCache) cl.loadClass(conf.getInstanceCache()).newInstance();
      }

  public class ConfigurationMetaData extends MetaData
   // set the instance cache
   instanceCache = getElementContent(getOptionalChild(element, "instance-cache"), instanceCache);

  <jboss.xml>
   <instance-cache>org.jboss.ejb.plugins.PerTxEntityINstanceCache



# ERROR

  17:07:29,417 WARN  [verifier] EJB spec violation:
  Bean   : Task
  Section: 22.2
  Warning: The Bean Provider must specify the
    fully-qualified name of the Java class that
    implements the enterprise bean's business
    methods in the <ejb-class> element.
  Info   : Class not found on
    'com.oreilly.jbossnotebook.todo.ejb.TaskCMP':
    Unexpected error during load of:
    com.oreilly.jbossnotebook.todo.ejb.TaskCMP,
    msg=com/oreilly/jbossnotebook/todo/ejb/TaskCMP
    (Unsupported major.minor version 49.0)

  原因：
    JBossASを動作させているjavaが、1.4.Xのときに、
    Java 1.5でEJBを作成してデプロイすると上記エラーが発生する。


# jboss.classloader.tips

include from doc/tips/jboss.classloader.tips

========================
Invokerの調査
========================
call by referenceの場合のinterceptor
  org.jboss.invocation.InvokerInterceptor
    setLocal( Invoker invoker );

  public class
  LocalInvoker
    extends ServiceMBeanSupport
    implements Invoker, LocalInvokerMBean

    void createService()
      InvokerInterceptor.setLocal( this );
      Registory.bind(serviceName, this );

=======================
ClassLoaderのクラス関係
=======================

public class
UnifiedClassLoader3
  extents UnifiedClassLoader
  implements UnifiedClassLoader3MBean

public class
UnifiedClassLoader
  extents RepositoryClassLoader
  implements UnifiedClassLoaderMBean, Translatable

public abstract class
RepositoryClassLoader
  extents URLClassLoader

  LoaderRepository repository;

  repositoryにtranslatorがセットされている場合は、
  transformしてクラスを作成する。

  protected Class findClass(String name) throws ClassNotFoundException
  {
    Translator translator = repository.getTranslator();
    if (translator != null)
    {
      // Obtain the transformed class bytecode 
      try
      {
        // Obtain the raw bytecode from the classpath
        URL classUrl = getClassURL(name);
        byte[] rawcode = loadByteCode(classUrl);

class
java.net.URLClassLoader
  extents SecureClassLoader

  このクラスローダは、JAR ファイルおよびディレクト
  リの両方を参照する URL の検索パスから、クラスお
  よびリソースをダウンロードするために使います。
  「/」で終わる URL は、ディレクトリを参照している
  と見なされます。その他の URL は、必要に応じて開
  かれる JAR ファイルを参照していると見なされます。 

  URLClassLoader のインスタンスを生成したスレッド
  の AccessControlContext は、そのあとにクラスおよ
  びリソースをロードするときに使われます。 

  ロードされるクラスには、デフォルトでは、
  URLClassLoader の作成時に指定された URL だけに接
  続できるアクセス権が与えられます。 

class
java.security.SecureClassLoader
  extents ClassLoader

abstract class
java.lang.ClassLoader

=======================
Repositoryのクラス関係
=======================
class
UnifiedLoaderRepository3
  extends LoaderRepository
  implements MBeanRegistration, NotificationEmitter,
             UnifiedLoaderRepository3MBean

abstract class
LoaderRepository
  implements ServerConstants, ClassLoaderRepository

  Translator translator;

  getTranslator()

interface Translator
  クラスファイルを転送して、新しいクラスを作成


TCL
  Thread context Class Loader


はじめに
 - jboss 3.2まではフラットなclass loadingモデルであった。 
   o 異なるレイヤーで重複してクラスを読みこまない
     ようにするためにこのようなモデルであった。 
   o WARはwebやサーブレット、ejbとそのinterface,impl
     やtypeなどを含むので、これらがフラットなものとし
     て扱われる。 
   o もし、バージョン管理などの理由でクラスファイル
     をシェアしたくない場合には、他のアプリケーショ
     ンとクラスのスコープを分ける必要がある。 

 - クラスのスコープには２つの方法がある。 
   - 他のデプロイメントからの独立 
   - サーバクラスの上書きによる独立 
 - top levelのデプロイメントだけがスコープを指定するかもしれません。 
 - EARの場合は、META-INF/jboss-app.xmlだけでスコープを指定できます。 
 - SARの場合は、META-INF/jboss-service.xmlだけでスコープを指定できます。 

webコンテナ
 JBoss323 
  - webアプリケーションのクラスローダーとしてUnified Class Loader(UCL)を使用している 
  - jbossweb.../META-INF/jboss-service.xmlのUseJBossWebLoader属性でコントロールしている 
  - UCLとは 
    - WAR内のWEB-INF/classes,WEB-INF/jarsにあるク
      ラスが、デフォルトshared calss loaderレポジトリに
      入れるもの 
    - WEBアプリケーションの中で、class,resourceをシェアする 
    - 上記属性をfalseにするとこの機能を無効に出来る。 
 JBoss4.0.2 
  - サーブレットスペックのクラスローディングを採用
    (Tomcat class loaderを採用) 

Class Loader Repository
  (org.jboss.mx.loading.UnifiedLoaderRepository3)
 - JBossでclassをシェアする仕組み
 - 複数のUCLに対応可能
 - 複数のUCLを対応付けることでUCL間でclassをshareする
 - calss cacheを保持
 - packages Mapを保持
 - 子供のRepository(HierarchialLoaderRepository)を
   持つことが可能
     org.jboss.mx.loading.HeirarchicalLoaderRepository3 
 - J2EEスタイルのclassネームスペースisolationを行
   なう場合は、新たにHierarchialLoaderRepositoryを
   作成し、それにアプリケーションを対応付けること
   で対応
  o アプリケーションは、UCLのclasspathからも、親の
    Repositoryからもclassをロード可能
  o これは、Java2ParentDelegationフラグに依存
  o アプリケーションは、親のRepositoryのcacheにア
    クセスすることも可能
  o しかし、兄弟の関係にあるRepository間では、
    絶対classをshareすることは出来ない。
 - HierarchialLoaderRepositoryのclass loaderは
   親より優先される。

UCL
  (org.jboss.mx.loading.UnifiedClassLoader3)
 - classをロードする仕組み
 - 1つのShared Repositoryに対応
 - classをロードしてくるためのURLを複数保持可能

 - アクセス可能なclassはURLで指定
 - 作成された時に、アクセス可能なclassのpackageの
   マップをShared Repository内のpackagesMap内に作成
  o packagesMapに登録される順番は重要!!!!!
 - 標準的なJava2 class loading modelをオーバーライ
   ドする
 - 検索順番
  o 親のRepositoryのcacheをチェック(getCachedClass())
    - 親のRepositoryのJava2ParentDelegationフラグ
      がonの場合は、親のRepositoryのcacheの内容を
      チェック(defultは、offなので親のrepositoryの
      内容はチェックしない)
  o 親のRepositoryのpackagesMapの内容からclass
    をload可能な最初のUCLを取得(getPackageClassLoaders())
  o 上記UCLにclassの取得を依頼(loadClassLocally())
  o 標準的なJava2の親に委譲(super.loadClass())
 - classが見付かった後には、対応するRepositoryの
   class cacheにclassを登録(repository.cacheLoadedClass())
 - $JAVA_HOME/libパッケージは、Repositoryの
   packagesMapには登録されないが、見付かったclass
   はcache内には登録される。

NoAnnotarionClassloader(NACL)
 - UCLは1つのNACLを親に持つ
 - SingletonのNACLがサーバブート時に作成される
 - $JBOSS_HOME/lib内で有効なclassが定義される
 - 親のクラスローダは、System Class Loader




  Java2ClassLoadingCompliance
    servlet 2.3のwebコンテナを最初に見るようなモデ
    ルではなく標準のjava2の最初のclass loadingモデ
    ルを使用するように設定
      default:false (Tomcatのwebコンテナを最初に見る)

    TomcatDeployer extends AbstractWebDeployer
      tomcat web application デプロイヤ
      MBeanServer server.setAttribute(objName, "delegate", Java2ClassLoadingCompliance);

    AbstractWebDeployer
      protected boolean java2ClassLoadingCompliance = false;


  UseJBossWebLoader
    JBoss Loaderを使用するように設定。このローダは、
    tomcat特有のclass loaderを使用するのではなく
    class loaderとしてUnifiedClassLoaderを使用
      default:false (Tomcatのclassloaderを使用)

    org.jboss.web.tomcat.tc5.TomcatDeployer
      extends AbstractWebDeployer

      ClassLoader loader = Thread.currentThread().getContextClassLoader();

      /* If we are using the jboss class loader we need to augment its path
      to include the WEB-INF/{lib,classes} dirs or else scoped class loading
      does not see the war level overrides. The call to setWarURL adds these
      paths to the deployment UCL.
      */
      Loader webLoader = null;
      if (config.isUseJBossWebLoader())
      {
         // JBossのtomcatローダを使用
         WebCtxLoader jbossLoader = new WebCtxLoader(loader);
         jbossLoader.setWarURL(url);
         webLoader = jbossLoader;
      }
      else
      {
         // WebAppLoaderは、tomcatのWebappLoaderをextendしたクラス
	 //   org.apache.catalina.loader.WebappLoader
	 // defaultのclass loaderをWebAppClassLoaderに設定
	 // filterされるパッケージをWebAppClassLoaderに渡す

         String[] pkgs = config.getFilteredPackages();
         WebAppLoader jbossLoader = new WebAppLoader(loader, pkgs);
         webLoader = jbossLoader;
      }
  

  package org.jboss.web.tomcat.tc5;
  WebCtxLoader
    implements Lifesycle, org.apache.catlina.Loader;

    WebCtxLoader(ClassLoader encLoader)
    {
      this.delegate = (RepositoryClassLoader)parent;
         ....encLoaderの親を辿ってJBossの
	     RepositoryClassLoaderを探してdelegate
	     先として委譲する。
    }

  package org.jboss.web.tomcat.tc5;
  WebAppLoader
    extends org.apache.catlina.loader.WebappLoader;


  UnifiedClassLoader
    - single URLからロードするクラスローダ
    - LoaderRepositoryと共に動作
    - repositoryとして独立して動作しない。

  UnifiedClassLoader3
    - UnifiedClassLoaderの拡張
    - multiスレッド環境で動作出来るように対応

  LoaderRepository
    LoaderRepositoryのabstractベースクラス

  UnifiedLoaderRepository3 extends LoaderRepository

    - class loaderのリポジトリ
    - classとリソースのネームスペースをフラットにする。
    - UnifiedClassLoader3インスタンスを使用する。
    - classとresourceローディングはrepositoryのモニタによって
      synchronizeされる。

  CLの階層構造
    Bootstrap CL
    System CL
    NoAnnotationURL CL
    UCL3

  HeirarchicalLoaderRepository3
    - UnifiedLoaderRepository3の拡張
    - classとリソースは最初に子供からロードされる。
    - 親は、java2ParentDelegation flagに依存する。

    - java2ParentDelegation
      classやリソースをロードするときに、親のリポジトリを
      最初に使うかどうかのフラグ

      UnifiedLoaderRepository3 parentRepository;

      true : 親のリポジトリを最初に使用
             parentRepository.loadClass(name, resolve, ClassLoader);
             無ければ
               super.loadClass(name, resolve, ClassLoader);
      false : HeirchicalLoaderRepositoryを最初に使用
             super.loadClass(name, resolve, ClassLoader);
             無ければ
               parentRepository.loadClass(name, resolve, ClassLoader);


!!! Enable Classloader Logging

!! Log4J

All JBoss logging is based on Log4j.  Classloader
logging is no different.  To enable logging for
classloaders, edit log4j.xml in the
$JBOSS_HOME/server/xxx/conf/log4j.xml and add 

{{{
    <appender name="UCL" class="org.apache.log4j.FileAppender">
        <param name="File" value="${jboss.server.home.dir}/log/ucl.log"/>
        <layout class="org.apache.log4j.PatternLayout">
            <param name="ConversionPattern" value="[%r,%c{1},%t] %m%n"/>
        </layout>
    </appender>

    <category name="org.jboss.mx.loading" additivity="false">
        <priority value="TRACE" class="org.jboss.logging.XLevel"/>
        <appender-ref ref="UCL"/>
    </category>
}}}

The additivity="false" attribute says to only send
the org.jboss.mx.loading category output to the
associated appender-ref. If you want output to go
to the server.log in addition to the ucl.log set
additivity="true". If you only want the output to
go to the server.log, set additivity="true" and
remove the appender-ref element. The UCL appender
creates a ucl.log in the /server/xxx/log directory
which will contain output such as:

Much of the output will be specific to the jboss
class loader implementation, but the main class
loading loadClass entry point will be shown. Other
key messages are the LoadMgr beginLoadTask,
nextTask and endLoadTask calls.

