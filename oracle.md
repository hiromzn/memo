# docs

- https://www.oracle.com/technetwork/jp/database/enterprise-edition/documentation/index.html

# config

- tnsnames.ora

  - search priority
    - $TNS_ADMIN/tnsnames.ora
    - /etc/tnsnames.ora
    - $ORACLE_HOME/network/admin/tnsnames.ora

# sqlplus

## login

```
sqlplus <user_id>/<pass>@<connect_identifier>
```

- connect_identifier is defined in tnanames.ora

## tips

```
$ sqlplus
Error 46 initializing SQL*Plus
HTTP proxy setting has incorrect value
SP2-1502: The HTTP proxy server specified by http_proxy is not accessible

$ env |grep prox
http_proxy=socks5h://localhost:3131
https_proxy=socks5h://localhost:3131

$ unset http_proxy
$ sqlplus

SQL*Plus: Release 19.0.0.0.0 - Production on  5 19 15:33:25 2023
Version 19.6.0.0.0
```

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

# SQL

## setup
```
-- CSV format
SET MARKUP CSV ON;

-- the separator character used to split your columns
set colsep ,

set headsep off

-- the number of lines “per page.”
set pagesize 0

set trimspool on
```

## execute script file

```
@file_name
```

## output the result to file

```
-- SQL*PLus will store the output of any query to the specified file
spool <file_path>
-- execute SQL...
spool off
```

## table
```
-- show all table
select table_name, owner from all_tables order by owner, table_name;
SELECT table_name FROM user_tables ORDER BY table_name;

SELECT table_name, owner FROM all_tables WHERE owner='schema_name' ORDER BY owner, table_name;
SELECT table_name, owner FROM all_tables WHERE table_name='%keyword%' ORDER BY owner, table_name;
SELECT table_name, owner FROM dba_tables WHERE owner='schema_name' ORDER BY owner, table_name;
```
