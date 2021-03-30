### shbang

```
#!/usr/bin/env bash

#!/usr/bin/bash
```


### editor

- コマンドライン編集モード

  ```
  set -o vi
  set -o emacs
  ```

### set

- set -e
  - exit if commands exits with non-zero status.

### regexpression

- perl: \w+  ==  sed: [^ ]\{1,\}

  ```
  $ cat in
  test   <
  testabc<
  testaaa<

  $ cat in |sed 's/[abc ]\{1,\}/ /g'
  test <
  test <
  test <
  ```

### shell path

- スクリプトが配置されている絶対パスの取得

  ```
  MYDIR="$(dirname $(which $0))";
  ```

  - bashであれば以下のような方法もある？

    ```
    #!/bin/bash
    ROOT=$( dirname $( readlink -f $0 ) )
    ```

- スクリプトが実行されたディレクトリの絶対パスの取得

  ```
  SCRIPT_DIR=$(cd $(dirname $0); pwd)
  ```

  - 注意：　PATHでサーチしたスクリプトの場合は、コマンドを呼び出したディレクトリ名となる。

### parameter replace

```
${parm<記号>word}
```

| 記号	| function |
|-------|----------|
| %		| 最短後置パターンの削除 |
| %%	| 最長後置パターンの削除 |
| #		| 最短前置パターンの削除 |
| ##	| 最長前置パターンの削除 |

### parameter length

```
${#parameter}
```

### environment

```
${parm<記号>word}
```

| 記号		| parm=NULL		| parm!=NULL		| 意味	|
|---------------|-----------------------|-----------------------|-------|
| :-		| 式=word, parm=NULL	| 式=parm, parm=parm	| デフォルト値への置換 |
| :=		| 式=word, parm=word	| 式=parm, parm=parm	| デフォルト値への代入 |
| :?		| error	   		| 式=parm, parm=parm	| 値の検査とエラー |
| :+		| 式=NULL, parm=NULL	| 式=word, parm=parm	| 代替値の使用 |

- ```${parameter:-word}```
  - prarmeterがNULLなら値はwordになる。
    ```
	$ echo ${FOO:-val}
	val
	$ echo $FOO
	<null>	
    ```

- ```${parameter:=word}```
  - parameterがNULLなら値はwordになり、parameterの値もwordになる。
    ```
	$ echo ${FOO:=val}
	val
	$ echo $FOO
	val
    ```
	

- ```${parameter:?word}```
  - parameterがNULLならerrorになる。
  - parameterに値があれば、parameterの値となる。
    ```
	$ echo ${FOO:-val}
	bash: FOO: val
	$ echo $FOO
	<null>
    ```
- ```${parameter:+word}```
  - parameterがNULLのとき、式の値はNULLになる。
  - parameterの値があるとき、式の値はwordの値になるが、parameterの値はそのまま。

- ${parameter:offset}
- ${parameter:offset:length}

## 配列

```
array=(111 "foo" 222 "bar" 333 "foobar")

array=("foo" "bar")
array[0]="foo"
array[1]="bar"

array+=("end");		# add element
array+=("123" "456");   # add two elements...

${array[0]}

index=3
echo ${array[$index]}

echo "${#array[*]}";	# show number of elements.
6

for i in "${array[@]}"
do
  ...
done
```

### redirect
- sample code

```
ls /foo >log	# redirect stdout only
ls /foo 2>log	# redirect stderr only
ls /foo &> log	# redirect stdout/stderr

ls /foo | cat >log	# pipe stdout only	
ls /foo |& cat >log	# pipe stdout/stderr
```

- teeコマンドを使って、stdout, stderrを別のファイルに記録する。

```
command > >(tee stdout.log) 2> >(tee stderr.log >&2)
```

```
comments:
	> >(..)

	>(...) (process substitution) creates a FIFO and lets tee listen on it. 
	Then, it uses > (file redirection) to redirect the STDOUT of command 
	to the FIFO that your first tee is listening on.

	http://stackoverflow.com/questions/692000/how-do-i-write-stderr-to-a-file-while-using-tee-with-a-pipe
```

### 文法チェック

  $ /bin/bash -n foo.sh
  $ /bin/sh -n foo.sh

  * 変数のタイプミスなどではエラーが出ない

### shebangの環境依存の回避

　シェルスクリプトを実行するコマンドのパスが環境によって異なる場合の実現方法

　#!/usr/bin/env sh

　（参考）http://ja.wikipedia.org/wiki/%E3%82%B7%E3%83%90%E3%83%B3_(Unix)



### ^Cで子供のプロセスを含めて終了させる方法

```
  main_pid=$$;

  trap trap_func SIGINT SIGQUIT;

  trap_func()
  {
    kill -SIGTERM -$main_pid
    exit 1
  }
```

### argumentの処理

```
  while getopts "vx:n" opt; do
    case $opt in
	v) verbose=1 ;;
	n) norun=1 ;;
	x) echo "ARGVER is $OPTARG" ;;
    esac
  done
```

```
while [ $# -gt 0 ]; do
    case "$1" in
    linux ) make_os="linux";;
    hpux | HPUX | HP-UX | ca ) make_os="hpux";;
    esac
    shift;
done
```

### 文字列の処理

- 左端の除去
  例) a=a/b/c/d
      echo ${a##*/} --> "d";
      echo ${a#*/} --> "b/c/d";

- 右端の除去
  例) a=a/b/c/d
      echo ${a%%/*} --> "a";
      echo ${a%/*} --> "a/b/c";

### 配列

```
[hmizuno@hpjcta bin]$ cat test.sh
#!/bin/sh

cat >>cmd_file <<EOF
cmd_file CVS
kobitokunJr.sh
kobitokun.sh
kobitosan.sh
test.sh
EOF

cmd_list=(`cat cmd_file`)

echo OUT;
echo ${cmd_list[0]};
echo ${cmd_list[1]};
echo ${cmd_list[2]};
echo ${cmd_list[3]};

echo XXXXXXXXXXXX

echo ${cmd_list[*]}

[hmizuno@hpjcta bin]$ ./test.sh
cmd_file
CVS
kobitokunJr.sh
kobitokun.sh

XXXXXXXXXXXX

cmd_file CVS kobitokunJr.sh kobitokun.sh kobitosan.sh test.sh

```
