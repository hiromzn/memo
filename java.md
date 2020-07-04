#### jcmd
```
# get java process Id
$ jcmd

# get java thread dump
$ jcmd <pid> Thread.print 

# get java heap dump
$ jcmd <pid> GC.heap_dump /fullpath/to/dump_file
```

##### command list

- VM.uptime 起動時間(秒)
- VM.flags 起動オプションの表示
- VM.flags -all 全オプションの値表示
- VM.system_properties Systemプロパティの表示
- VM.command_line 起動時のコマンドライン表示
- VM.version バージョン表示
- GC.run System.gc()の実行
- GC.run_finalization System#runFinalization()の実行
- GC.class_histogram ヒープ上のインスタンス数、バイト数出力

#### hp jvm

  -Xmpas:on
    "java_q4p" を強制的に起動することが出来る。
    (-Xmx の値が小さくても "java_q4p" を起動することが出来ます)。

    [-Xmpas:on が使用できる JavaVM のバージョン]
      1.4.2.06～
      1.5.0.00～

    -Xmpas:on オプションが追加されているバージョンであれば、
    java のヘルプには以下のように表示されます。

    $ java -X
    .....
      -Xmpas:on         enable HPUX MPAS addressing mode, otherwise fail
      -Xmpas:off        do not attempt to enable HPUX MPAS addressing mode
### GC option

  -verbose:gc

 HPUX
  -Xverbosegc:[0|1][:file=[stdout|stderr|<filename>]]

  0|1 controls the printing of heap information:
        0   After every Old Generation GC or Full GC
        1   (default) After every GC

  :file=[stdout|stderr|<filename>] specifies output file
      stderr    (default) directs output to standard error stream
      stdout     directs output to standard output stream
      <filename> file to which the output will be written

perm sizze
  -XX:MaxPermSize=<size>

JVM memory size
  -Xmpas:onを指定すると、JVMプロセスは4Gbyteのメモリ空間を使用する。
  指定しないと、Java Heapサイズに依存して使用メモリ空間を制限する。

  http://h50221.www5.hp.com/cgi/service/knavi/production/doc_disp.cgi?category=1141&doc=jnav000398

  http://docs.hp.com/en/JAVAPROGUIDE/expanding_memory.html

HP jvm eprof

  $ java -Xeprof:time_on=sigusr2,time_slice=sigusr2,inlining=disable,file=/tmp/myap ...

  $ kill -SIGUSR2 <pid_of_java>   ## data取得開始
  $ kill -SIGUSR2 <pid_of_java>   ## data取得終了

  -Xmpas:onオプションの追加

  -Xeprof usage: -Xeprof[:help]|[:<key>[=<value>],<key> ...]

  Key           Description
  -------------------------------------------
  help          print this help message
  time_on=<integer>
                specifies time in seconds after which profiling will start
  time_on=sigusr1|sigusr2
                specifies which signal will cause profiling start
  time_slice=<integer>
                specifies time in seconds since profiling start after which
                profiling will be terminated and the data will be written out
  time_slice=sigusr1|sigusr2
                specifies which signal will cause profiling termination
  inlining=disable|enable
                disabling the method inlining by the compiler helps to obtain
                profile data for methods that would normally get inlined
               (default is enable)
  ie=yes|no     enable/disable profiling intrusion estimation (default=yes)
  file=<file>   write profile data to <file> (default is java<pid>.eprof)

                if no -Xeprof option is specified, it is equivalent to:
                -Xeprof:time_on=sigusr2,time_slice=sigusr2

thrad dump
  $ kill -QUIT <pid>

GC option
  -XX:+UseSerialGC ...... シリアルGC
  -XX:+UseParallelGC .... パラレルコレクタ

vm options
  http://java.sun.com/javase/technologies/hotspot/vmoptions.jsp

  ParNew : Young generation (ParNew) collection.
  DefNew : New 領域に対するGC
  
sleep
  try {
    Thread.sleep( msec );
  } catch( Exception e ) {}

時間
  print.. new Date();
    --> Wed Jul 26 11:56:47 GMT+09:00 2006

  print.. System.currentTimeMillis();

map, HashMap
             同期化
  HashTable: ○
  HashMap:   △
  HashTable: △

  ConcurrentHashMapは、並列な更新が実施できるHashMap

  ※ △ .... 実装はしていないが、以下により同期化を実施する。

             Map syncMap = Collections.synchronizedMap(new HashMap(...));

  synchronizedMapと、ConcurrentHashMapの違いについて
    - http://www.ibm.com/developerworks/jp/java/library/j-jtp07233/index.html
    --> java.hashmap.concurrent.tips

Runtime
  .maxMemory()
  .totalMemory()
  .freeMemory()
  .gc()
  .runFinalization()

array
  String[] name = new String[3];
  String[] str  = {"ABC", "DEF"};

  int[] i = {1,2,3};
  int[] i = new int[3];

hprof option
Hprof usage: -Xrunhprof[:help]|[:<option>=<value>, ...]

Option Name and Value  Description                Default
---------------------  ----------------------     -------
heap=dump|sites|all    heap profiling             all
cpu=samples|times|old  CPU usage                  off
monitor=y|n            monitor contention         n
format=a|b             ascii or binary output     a
file=<file>            write data to file         java.hprof(.txt for ascii)
net=<host>:<port>      send data over a socket    write to file
depth=<size>           stack trace depth          4
cutoff=<value>         output cutoff point        0.0001
lineno=y|n             line number in traces?     y
thread=y|n             thread in traces?          n
doe=y|n                dump on exit?              y
gc_okay=y|n            GC okay during sampling    y

Example: java -Xrunhprof:cpu=samples,file=log.txt,depth=3 FooClass

Note: format=b cannot be used with cpu=old|times

property
  propertyファイルからの読み込み
    message.properties:
      msg=Hello

    Properties prop = new Properties();
    prop.load(new FileInputStream("message.properties"));
    String msg = prop.getProperty("msg");
    System.out.println(msg); // print Hello !!!

数字の文字列からintへの変換
  int i = Integer.parseInt( String );

SJIS codeの文字の設定
  byte[] sjisCode = new byte[] { (byte)0x81, (byte)0x5c};
  String str = new String( sjisCode, "SJIS" );

LANG
  LANG=XXXX java ...
  java -Dfile.encoding=ja_JP.SJIS 

TS-2289 Reflection - Java Technology's Secret Weapon

  Reflection は java.lang.reflect.Member インタフェー
  スを親とする Field, Method, Constructor クラスと
  InvocationHandler インタフェースを親とする Proxy
  クラス、また Proxy クラスの派生クラスの Array,
  Modifier クラスが中心になっています。また、
  java.lang.reflect パッケージではありませんが、
  java.lang.Class があります。

  Class オブジェクトを得るには次の 4 種類の方法があります。

  - Class.forName メソッド 
  - Object クラスの class プロパティ (e.g. String.class or int.class) 
  - Object.getClass メソッド 
  - プリミティブのラッパクラスの TYPE 定数 (e.g. Integer.TYPE) 

  Field, Method, Construct などはすべて Class オブジェクトから得ることができます。

  Reflection を使うときに考慮すべき点には次の 3 点があります。

  - Readable 
  - Performance 
  - Code Size 

  Reflection を使うとどうしても Readability が低下
  します。Readability をすこしでも向上するためには
  ユーティリティメソッドなどを使用して直接
  Reflection をコールする部分をラッパしてしまう方
  法があります。

  次の Performance ですが、J2SE v1.4 以前は
  Reflection を使用するとかなり Performance が落ち
  ることが知られていました。しかし、J2SE v1.4 では
  HotSpot の最適化により、直接メソッドをコールした
  場合と Reflection を使用した場合はそれほど変わら
  なくなりました。

  しかし、Method オブジェクトを得る処理に時間がか
  かるのは以前も今も変わりません。そこで、一度
  Method オブジェクトを得たら、それをキャッシュし
  ておくことで Performance 低下を防ぐことができま
  す。

  最後の Code Size ですが、Reflection を使うことで
  コードのサイズを減らすこともできます。たとえば
  GUI で多数のボタンが配置されており、それぞれイベ
  ントの処理が異なるとします。一般にこのようなとき
  には Inner Class を使用して次のように記述するこ
  とが多くあります。

    button1.addActionListener(new ActionListener() {
        public void actionPerformed(ActionEvent event) {
            doSomething1();
        }
    }
 
  このコードだと n 個のボタンがあったときには ｎ
  個の Inner Class ができてしまいます。そこで、次
  のような ActionListener を作成します。

    public class ReflectionActionAdapter implments ActionListener {
        private Method method;
        private Object obj;
 
        public ReflectionActionAdapter(Method method, Object obj) {
            this.method = method;
            this.obj = obj;
        }
 
        public void actionPerformed(ActionEvent event) {
            method.invoke(obj, null);
        }
    }
 
  この方法だと ReflectionActionAdapter クラスが 1
  つできるだけで、このクラスのオブジェクトが複数生
  成されます。したがって、全体的なコードのサイズを
  減らすことが可能になります。

  Reflection がよく使われる場合を次に示します。

  Factory Method 
  InterPreter 
  Double Dispatch 
  Interposition 

  最後の Interposition はあるオブジェクトのメソッ
  ドコールをフックして他の処理をいれたいときなどに
  使用します。Servlet 2.3 のフィルターと同じような
  ことを行うわけです。これには Proxy クラスを使用
  して Dynamic Proxy を作成することで可能になりま
  す。


  Reflection を有効に使った例としては

  Tomcat server.xml を使用した初期化に Reflection を使用 
  jEdit JRE のバージョンをチェックし、処理方法を変化させるために使用 

  などがあります。

  最後に Reflection では型の情報がないため Type
  Safty を望むのであれば、Reflection は使うべきで
  はないことを付け加えておきます。


