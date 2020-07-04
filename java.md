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

- VM.uptime �N������(�b)
- VM.flags �N���I�v�V�����̕\��
- VM.flags -all �S�I�v�V�����̒l�\��
- VM.system_properties System�v���p�e�B�̕\��
- VM.command_line �N�����̃R�}���h���C���\��
- VM.version �o�[�W�����\��
- GC.run System.gc()�̎��s
- GC.run_finalization System#runFinalization()�̎��s
- GC.class_histogram �q�[�v��̃C���X�^���X���A�o�C�g���o��

#### hp jvm

  -Xmpas:on
    "java_q4p" �������I�ɋN�����邱�Ƃ��o����B
    (-Xmx �̒l���������Ă� "java_q4p" ���N�����邱�Ƃ��o���܂�)�B

    [-Xmpas:on ���g�p�ł��� JavaVM �̃o�[�W����]
      1.4.2.06�`
      1.5.0.00�`

    -Xmpas:on �I�v�V�������ǉ�����Ă���o�[�W�����ł���΁A
    java �̃w���v�ɂ͈ȉ��̂悤�ɕ\������܂��B

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
  -Xmpas:on���w�肷��ƁAJVM�v���Z�X��4Gbyte�̃�������Ԃ��g�p����B
  �w�肵�Ȃ��ƁAJava Heap�T�C�Y�Ɉˑ����Ďg�p��������Ԃ𐧌�����B

  http://h50221.www5.hp.com/cgi/service/knavi/production/doc_disp.cgi?category=1141&doc=jnav000398

  http://docs.hp.com/en/JAVAPROGUIDE/expanding_memory.html

HP jvm eprof

  $ java -Xeprof:time_on=sigusr2,time_slice=sigusr2,inlining=disable,file=/tmp/myap ...

  $ kill -SIGUSR2 <pid_of_java>   ## data�擾�J�n
  $ kill -SIGUSR2 <pid_of_java>   ## data�擾�I��

  -Xmpas:on�I�v�V�����̒ǉ�

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
  -XX:+UseSerialGC ...... �V���A��GC
  -XX:+UseParallelGC .... �p�������R���N�^

vm options
  http://java.sun.com/javase/technologies/hotspot/vmoptions.jsp

  ParNew : Young generation (ParNew) collection.
  DefNew : New �̈�ɑ΂���GC
  
sleep
  try {
    Thread.sleep( msec );
  } catch( Exception e ) {}

����
  print.. new Date();
    --> Wed Jul 26 11:56:47 GMT+09:00 2006

  print.. System.currentTimeMillis();

map, HashMap
             ������
  HashTable: ��
  HashMap:   ��
  HashTable: ��

  ConcurrentHashMap�́A����ȍX�V�����{�ł���HashMap

  �� �� .... �����͂��Ă��Ȃ����A�ȉ��ɂ�蓯���������{����B

             Map syncMap = Collections.synchronizedMap(new HashMap(...));

  synchronizedMap�ƁAConcurrentHashMap�̈Ⴂ�ɂ���
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
  property�t�@�C������̓ǂݍ���
    message.properties:
      msg=Hello

    Properties prop = new Properties();
    prop.load(new FileInputStream("message.properties"));
    String msg = prop.getProperty("msg");
    System.out.println(msg); // print Hello !!!

�����̕����񂩂�int�ւ̕ϊ�
  int i = Integer.parseInt( String );

SJIS code�̕����̐ݒ�
  byte[] sjisCode = new byte[] { (byte)0x81, (byte)0x5c};
  String str = new String( sjisCode, "SJIS" );

LANG
  LANG=XXXX java ...
  java -Dfile.encoding=ja_JP.SJIS 

TS-2289 Reflection - Java Technology's Secret Weapon

  Reflection �� java.lang.reflect.Member �C���^�t�F�[
  �X��e�Ƃ��� Field, Method, Constructor �N���X��
  InvocationHandler �C���^�t�F�[�X��e�Ƃ��� Proxy
  �N���X�A�܂� Proxy �N���X�̔h���N���X�� Array,
  Modifier �N���X�����S�ɂȂ��Ă��܂��B�܂��A
  java.lang.reflect �p�b�P�[�W�ł͂���܂��񂪁A
  java.lang.Class ������܂��B

  Class �I�u�W�F�N�g�𓾂�ɂ͎��� 4 ��ނ̕��@������܂��B

  - Class.forName ���\�b�h 
  - Object �N���X�� class �v���p�e�B (e.g. String.class or int.class) 
  - Object.getClass ���\�b�h 
  - �v���~�e�B�u�̃��b�p�N���X�� TYPE �萔 (e.g. Integer.TYPE) 

  Field, Method, Construct �Ȃǂ͂��ׂ� Class �I�u�W�F�N�g���瓾�邱�Ƃ��ł��܂��B

  Reflection ���g���Ƃ��ɍl�����ׂ��_�ɂ͎��� 3 �_������܂��B

  - Readable 
  - Performance 
  - Code Size 

  Reflection ���g���Ƃǂ����Ă� Readability ���ቺ
  ���܂��BReadability ���������ł����シ�邽�߂ɂ�
  ���[�e�B���e�B���\�b�h�Ȃǂ��g�p���Ē���
  Reflection ���R�[�����镔�������b�p���Ă��܂���
  �@������܂��B

  ���� Performance �ł����AJ2SE v1.4 �ȑO��
  Reflection ���g�p����Ƃ��Ȃ� Performance ������
  �邱�Ƃ��m���Ă��܂����B�������AJ2SE v1.4 �ł�
  HotSpot �̍œK���ɂ��A���ڃ��\�b�h���R�[������
  �ꍇ�� Reflection ���g�p�����ꍇ�͂���قǕς��
  �Ȃ��Ȃ�܂����B

  �������AMethod �I�u�W�F�N�g�𓾂鏈���Ɏ��Ԃ���
  ����͈̂ȑO�������ς��܂���B�����ŁA��x
  Method �I�u�W�F�N�g�𓾂���A������L���b�V����
  �Ă������Ƃ� Performance �ቺ��h�����Ƃ��ł���
  ���B

  �Ō�� Code Size �ł����AReflection ���g�����Ƃ�
  �R�[�h�̃T�C�Y�����炷���Ƃ��ł��܂��B���Ƃ���
  GUI �ő����̃{�^�����z�u����Ă���A���ꂼ��C�x
  ���g�̏������قȂ�Ƃ��܂��B��ʂɂ��̂悤�ȂƂ�
  �ɂ� Inner Class ���g�p���Ď��̂悤�ɋL�q���邱
  �Ƃ���������܂��B

    button1.addActionListener(new ActionListener() {
        public void actionPerformed(ActionEvent event) {
            doSomething1();
        }
    }
 
  ���̃R�[�h���� n �̃{�^�����������Ƃ��ɂ� ��
  �� Inner Class ���ł��Ă��܂��܂��B�����ŁA��
  �̂悤�� ActionListener ���쐬���܂��B

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
 
  ���̕��@���� ReflectionActionAdapter �N���X�� 1
  �ł��邾���ŁA���̃N���X�̃I�u�W�F�N�g��������
  ������܂��B���������āA�S�̓I�ȃR�[�h�̃T�C�Y��
  ���炷���Ƃ��\�ɂȂ�܂��B

  Reflection ���悭�g����ꍇ�����Ɏ����܂��B

  Factory Method 
  InterPreter 
  Double Dispatch 
  Interposition 

  �Ō�� Interposition �͂���I�u�W�F�N�g�̃��\�b
  �h�R�[�����t�b�N���đ��̏��������ꂽ���Ƃ��Ȃǂ�
  �g�p���܂��BServlet 2.3 �̃t�B���^�[�Ɠ����悤��
  ���Ƃ��s���킯�ł��B����ɂ� Proxy �N���X���g�p
  ���� Dynamic Proxy ���쐬���邱�Ƃŉ\�ɂȂ��
  ���B


  Reflection ��L���Ɏg������Ƃ��Ă�

  Tomcat server.xml ���g�p������������ Reflection ���g�p 
  jEdit JRE �̃o�[�W�������`�F�b�N���A�������@��ω������邽�߂Ɏg�p 

  �Ȃǂ�����܂��B

  �Ō�� Reflection �ł͌^�̏�񂪂Ȃ����� Type
  Safty ��]�ނ̂ł���΁AReflection �͎g���ׂ���
  �͂Ȃ����Ƃ�t�������Ă����܂��B


