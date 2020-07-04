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

  - オプションの値は、優先順位の低いものから順に、次のように決定される。

    - プリコンパイラに組み込まれている値
    - Pro*C/C++システム構成ファイル内の値の集合
    - Pro*C/C++ユーザー構成ファイル内の値の集合
    - コマンドラインで設定される値
    - インラインで設定される値

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

