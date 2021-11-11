# language tools

- Compiler Explorer

  - interactive compiler (source code -> assembly code)
  - https://godbolt.org/
  - https://github.com/compiler-explorer/compiler-explorer

### tool list for linux

```
$ yum history
ID     | Command line             | Date and time    | Action(s)      | Altered
-------------------------------------------------------------------------------
    44 | install tmux             | 2020-11-05 17:25 | Install        |    2   
    43 | install clang-devel      | 2020-10-22 13:34 | Install        |    1   
    42 | install clang            | 2020-10-22 13:33 | Install        |    3   
    41 | install tcsh             | 2020-10-15 09:35 | Install        |    1   
    40 | install devtoolset-7     | 2020-08-10 10:37 | Install        |   29   
    39 | install devtoolset-9     | 2020-08-08 14:28 | I, U           |   50   
    38 | install centos-release-s | 2020-08-08 14:26 | Install        |    2   
    37 | install scl-utils        | 2020-08-08 14:26 | Install        |    1   
    36 | install gcc-gfortran     | 2020-08-08 10:54 | Install        |    4   
    35 | install yum-utils        | 2020-08-06 15:45 | I, U           |    5   
    34 | install p7zip            | 2020-08-06 15:19 | Install        |    1   
    33 | erase autoconf           | 2020-07-15 11:31 | Erase          |    1   
    32 | install autoconf         | 2020-07-15 10:55 | Install        |    3   
    31 | install gdb              | 2020-07-15 09:44 | Install        |    1   
    30 | install nodejs           | 2020-07-14 15:45 | Install        |    1   
    29 | erase nodejs             | 2020-07-14 15:44 | Erase          |    2   
    28 | install https://rpm.node | 2020-07-14 15:42 | Install        |    1   
    27 | install nodejs           | 2020-07-14 14:30 | Install        |    3   
    26 | install mlocate          | 2020-07-07 17:45 | Install        |    1   
    25 | install libstdc++.i686   | 2020-07-03 23:45 | Install        |    2
```

### cloc

- summary
  - Count, or compute differences of, lines of source code and comments.

- operation sample

```
$ cloc --categorized=/tmp/CCC --by-file-by-lang  --force-lang=C,pc * 
     765 text files.
     716 unique files.
     327 files ignored.

github.com/AlDanial/cloc v 1.90  T=0.60 s (854.8 files/s, 325438.2 lines/s)
------------------------------------------------------------------------------------------
File                                                   blank        comment           code
------------------------------------------------------------------------------------------
MOD/foo/SRC/bar.c					                      182            180           3533
  :
COMMON/INC/ccc.h                                          28             54              0
------------------------------------------------------------------------------------------
SUM:                                                   20923          44353         130038
------------------------------------------------------------------------------------------

-------------------------------------------------------------------------------
Language                     files          blank        comment           code
-------------------------------------------------------------------------------
C                              268          13777          30410          94471
Bourne Shell                   210           6490          12536          24643
make                            11            292             47           4667
C/C++ Header                    17            303           1218           2444
diff                             2              0            142           2272
SQL                              5             61              0           1541
-------------------------------------------------------------------------------
SUM:                           513          20923          44353         130038
-------------------------------------------------------------------------------
# categorized file sample:
# % head /tmp/CCC
# 160282,(unknown),server-spec.pdf
# 30976,C,MOD/foo/SRC/bar.pc
# 3224,C/C++ Header,COMMON/INC/CA.h
# 3387,C/C++ Header,COMMON/INC/CC.h
# 13495,C/C++ Header,COMMON/INC/bb.h
# 10508,(unknown),COMMON/INC/CD.DB10
# 10552,(unknown),COMMON/INC/CD.DB20
# 13452,C/C++ Header,COMMON/INC/124.h
# 4761,C/C++ Header,COMMON/INC/aa.h
```
### tmux

- terminal multiplexer
- URL : https://github.com/tmux/tmux/wiki
- how to use
  ```
  $ tmux
  ```

- commands
  - prefix : Ctrl+b (default)

  ```
  prefix + ?	キーバインド一覧
  prefix + s	セッションの一覧表示
  prefix + c	新規ウィンドウ作成・追加
  prefix + w	ウィンドウの一覧
  prefix + &	ウインドウの破棄
  prefix + n	次のウインドウへ移動
  prefix + p	前のウインドウへ移動
  prefix + |	左右にペイン分割
  prefix + -	上下にペイン分割
  prefix + h	左のペインに移動
  prefix + j	下のペインに移動
  prefix + k	上のペインに移動
  prefix + l	右のペインに移動
  prefix + H	ペインを左にリサイズ
  prefix + J	ペインを下にリサイズ
  prefix + K	ペインを上にリサイズ
  prefix + L	ペインを右にリサイズ
  prefix + x	ペインの破棄
  prefix + スペース	ペインのレイアウト変更
  prefix + Ctrl + o	ペインの入れ替え
  prefix + {	ペインの入れ替え(上方向)
  prefix + }	ペインの入れ替え(下方向)
  prefix + [	コピーモードの開始(カーソルキーで移動)
  prefix + v	コピー開始位置決定(viモード)
  prefix + y	コピー終了位置決定(viモード)
  prefix + Crtl + p	コピー内容の貼り付け
  ```

### cscope

- https://github.com/tsuyopon/memo/blob/master/DEVTOOLS/Cscope.md
- tag jump tool for C / C++ / Java / PHP4
- has plugin for vim

### gnu global
- https://www.gnu.org/software/global/

- basic operation
  
### sql migrate
- https://github.com/rubenv/sql-migrate
	- Atomic migrations
	- Supports SQLite, PostgreSQL, MySQL, MSSQL and Oracle databases

### PDF tool
- CutePDF
- クセロPDF

### benchmark tool
- hdbench clone

### find command
- ```
	find / \( -user foo -o -name \*oya\* \) -printf "%b %p\n" \
      |awk '{ if ($1 > 8192) print $2; }' |xargs rm -rf;

    %b ..... file block size.
	```
### ieHTTPHeaders
- IEに組み込んで、接続先サーバーのホスト名、Referer、Cookieの内容などを確認可能

### valgrind
- https://valgrind.org/
- linux memory leak check tool
- Valgrind is an instrumentation framework for building dynamic analysis tools.
```
valgrind --tool=memcheck --leak-check=full --log-file=/tmp/valgrind.log hoge.exe
```

### libevent (http://monkey.org/~provos/libevent/)
- The libevent API provides a mechanism to execute a callback function
	when a specific event occurs on a file descriptor or after a timeout
	has been reached. Furthermore, libevent also support callbacks
	due to signals or regular timeouts.
