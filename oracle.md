# docs

- https://www.oracle.com/technetwork/jp/database/enterprise-edition/documentation/index.html

# proc

- usage

```
proc option=value...
```

- sample

```
proc myfile
proc INAME=myproc

```

- priority of option

  - ���ץ������ͤϡ�ͥ���̤��㤤��Τ����ˡ����Τ褦�˷��ꤵ��롣

    - �ץꥳ��ѥ�����Ȥ߹��ޤ�Ƥ�����
    - Pro*C/C++�����ƥ๽���ե���������ͤν���
    - Pro*C/C++�桼���������ե���������ͤν���
    - ���ޥ�ɥ饤������ꤵ�����
    - ����饤������ꤵ�����

# show options

```
proc 
proc config=my_config_file ?
proc maxopencursors=? 
proc ?
``` 

## pcscfg.cfg

- compiler option file

- sample

```
sys_include=/ade/aime_rdbms_9819/oracle/precomp/public 
sys_include=/usr/include,/usr/lib/gcc-lib/i486-suse-linux/2.95.3/include 
sys_include=/usr/lib/gcc-lib/i386-redhat-linux/3.2.3/include
sys_include=/usr/lib/gcc-lib/i386-redhat-linux7/2.96/include
sys_include=/usr/include
```

