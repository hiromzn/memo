# manual

- http://jtdan.com/vcs/kahori.com/j-cvsbook/j-cvsbook.html

### 環境変数の設定

```
##############
# local REPO
##############
CVSROOT=/data/cvsrepo

##############
# remote REPO
##############

#----------------
#　sshによる接続
#
CVS_RSH=ssh
CVSROOT=:ext:hmizuno@16.146.89.147:/data/cvsrepo
export CVSROOT CVS_RSH
#
#　補足：
#　　この場合、パスワードなしでssh接続できるように設定する
#
# $ ssh-keygen -t rsa -C "your_email@example.com"
#   push <RETURN> X 3
# $ ssh-copy-id -i $HOME/.ssh/id_rsa.pub <target_host>
#

#-------------------
#　pserverによる接続
#
# connect by pserver : run "cvs login" command before cvs operation
#
CVSROOT=:pserver:hmizuno@hpjsdabj.kobe.hp.com:/usr/local/opensource/cvsrepo
CVSROOT=:pserver:hmizuno@16.146.89.147:/data/cvsrepo
```

### CVSROOTの初期化
```
# export CVSROOT=/foo
# cvs init
```

### cvsへの初期登録（新規プロジェクトの登録）

	```
	# ソースの入っているトップディレクトリに移動
	$ cd $TOPDIR
	#　カレントディレクトリ以下を<name>で登録する。
	$ cvs import -m 'initial message' <name> vendor-tag release-tag

	$ cvs status -v
	cvs status: Examining test1
	===================================================================
	File: a                 Status: Up-to-date

	Working revision:    1.3     Sat Jan 20 11:35:18 2018
	Repository revision: 1.3     /cygdrive/d/work/cvs/root/test1/a,v
	Sticky Tag:          (none)
	Sticky Date:         (none)
	Sticky Options:      (none)

	Existing Tags:
        tag1                            (revision: 1.2)
        release-tag                     (revision: 1.1.1.1)
        vendor-tag                      (branch: 1.1.1)
	```

	```
	$ cvs login
	$ cd myproj_dir/
	$ cvs import -m "initial version" proj_name vendortag releasetag
		カレントディレクトリ以下をチェックイン
		proj_name .... CVSのproject名(coした時のディレクトリ名)
			($CVSROOTからみたパス）
		vendortag ....
		releasetag ...
	```

### サンプルオペレーション

  ```
  $ export CVSROOT=:pserver:<user>@16.161.40.195:/smcroot/cvs_smc
  $ cvs login
  password: xxxxx
  $ cd $WORK_DIR
  $ cvs co <package or project_name>
  ```

```
# list module 一覧表示
$ cd /tmp
$ cvs co -l -d temp .
$ cd temp
$ cvs -n up -d
```

### checkout( co / export )

  ```
  $ cvs co <proj_name>
  $ cvs co -r <tag> <pname>
  $ cvs co -r <tag> -d <dir_name> <pname>

  # CVSディレクトリを作成しないでソースだけを取り出す
  $ cvs export -r <<tag>> <pname>           # tag指定で取り出す
  $ cvs export -D now <pname>               # 最新のソースを取り出す
  $ CVSREAD=yes cvs export -r <tag> <pname> # fileをREADONLYにして取り出す
  ```
  
### 通常の編集作業
  ```
  $ cvs up ..... repositoyと現在のチェックアプトしたディレクトリの内容の違いを確認
  $ vi *.c
  $ cvs up

  $ cvs diff　。。。。レポジトリーの内容との差分を表示


  $ cvs commit　。。。　修正をレポジとリーに登録
    or
  $ cvs commit -m "message"
    or
  $ cvs commit -m "message" <fileA> <fileB>
  ```

### working directory 状態確認

```
$ cvs status
# cvs co <projname> : create command
cvs status: Examining test1
===================================================================
File: a                 Status: Up-to-date
   Sticky Tag:          (none) <<< 編集可能
   Sticky Date:         (none)
   Sticky Options:      (none)

# cvs co -r branch_tag -d test1_brag_p1 <projname> : create command
cvs status: Examining test1_btag_p1
===================================================================
File: a                 Status: Up-to-date
   Sticky Tag:          btag_p1 (branch: 1.2.2)　<<< branchなので編集可能
   Sticky Date:         (none)
   Sticky Options:      (none)

# cvs co -r revision_tag -d test1_tag1 <projname> : create command
cvs status: Examining test1_tag1
===================================================================
File: a                 Status: Up-to-date
   Sticky Tag:          tag1 (revision: 1.2)　<<< revisionなので編集できない
   Sticky Date:         (none)
   Sticky Options:      (none)
```

### TAG

  ```
	$ cvs tag TAG_NAME files....	  : タグを付ける
	$ cvs co -r TAG_NAME project_name : タグを指定したチェックアウト
	$ cvs tag -d TAG_NAME			  :　タグを削除(delete)する。
	$ cvs status -v backend.c         :　タグリストの取得
  ```

#### TAG name rule

- ブランチ用のタグ：
  - BP_baseBranchTagName_newBranchNAme
	- ブランチの元のソースに打つタグ名（BTAGの元となった場所を示す）
  - BT_baseBranchTagName_newBranchNAme
	- 新しいブランチのブランチタグ名

- ブランチ用のタグ：
	- BPOINT_patch01 ...... ブランチの元のソースに打つタグ名（BTAGの元となった場所を示す）
	- BTAG_patc01 ........ 新しいブランチのブランチタグ名

- マージ用のタグ：     
	- MPOINT_BTAG_patch01 ...... マージの元となるソースに打   つタグ名
	- MTAG_BTAG_patch01_before.. マージ先のソースに打つタグ名（マージ直前に打つ）
	- MTAG_BTAG_patch01 ........ マージ先のソースに打つタグ名（マージ後に打つ）


						# BRANCH operation #		# MARGE operation 01 #			# MARGE operation 02 #

												[MTAG_BTAG_patch01-01_before]	[MTAG_BTAG_patch01-02_before]
		<HEAD> ........ [REV_1_0] ............... [MTAG_BTAG_patch01-01] .........[MTAG_BTAG_patch01-02]
						[BPOINT_patch01]			A								A
						|							|								|
						v							|								|
					<BTAG_patch01>................[MPOINT_BTAG_patch01-01]........[MPOINT_BTAG_patch01-02]

### file 操作

#### add file

```
cvs add 
```
### branch 操作

##### create branch
```
# 新しいブランチの元となるブランチ（HEADなど）で実施
$ cvs tag REV_1_0
$ cvs tag BPOINT_patch01
$ cvs tag -b BTAG_patch01   <<<<< create branch
```

##### check branch name
```
$ cvs status
Stickey tag:    <none>    <<<<< HAED branch
Stickey tag:    BTAG_patch01 (branch: 1.3.2) <<<< branch "BTAG_patch01"
```

##### create branch working dir
```
$ cvs co -d BTAG_patch01 -r BTAG_patch01 <project_name>
```

##### change branch
```
$ cvs update -r BTAG_patch01
```

##### delete branch
```
$ cvs tag -d -B <branch_tag>
```

##### marge
```
# BRANCHで実施
$ cvs co -r BTAG_patch01 <project_name>
$ cvs tag MPOINT_BTAG_patch01-01

# HEADで実施
$ cvs co <project_name>
$ cvs tag MTAG_BTAG_patch01-01_before
$ cvs update -j MPOINT_BTAG_patch01-01

# conflictが発生したら
$ vi <conflict files>

$ cvs ci -m "marge MPOINT_BTAG_patch01-01"
$ cvs tag MTAG_BTAG_patch01-01
```

##### 2度目のmarge
```
# BRANCHで実施
$ cvs co -r BTAG_patch01 <project_name>
$ cvs tag MPOINT_BTAG_patch01-02

# HEADで実施
$ cvs co <project_name>
$ cvs tag MTAG_BTAG_patch01-02_before
$ cvs update -j MPOINT_BTAG_patch01-01 -j MPOINT_BTAG_patch01-02

  # conflictが発生したら
  $ vi <conflict files>

$ cvs ci -m "marge MPOINT_BTAG_patch01-02"
$ cvs tag MTAG_BTAG_patch01-02
```

### タグ
- http://vega.sra-tohoku.co.jp/~kabe/vsd/cvs/cvsbranch.html

- タグ
		ある時点で cvs tag REL_1_0 とタグを打つと、 その後 -rREL_1_0 でcheckoutしたりすれば、いつでも常に同じものがとり出されます。タグは、普通は cvs tag POINT_TAG で打ちます。

- ブランチタグ
		一定のRCS版番は指さず、その時点での枝の先端 (成長点) の版を指します。

		具体的には、枝タグ自体は 1.5.2 といった数字が奇数個の版番に割り当てられ、checkout時などに自動的に先端を指すように最後の数字が補完されます。

			【例】main.c の RCS枝 1.5.2.x に対し、REL_1_0_1 と枝タグがついていれば、-rREL_1_0_1 などはその時点でリポジトリに入っている 1.5.2.x の 最新版(たとえば 1.5.2.6) に対する操作になります。

		cvs commit にて伸ばすことができるのは枝の成長点だけである。
		「幹の1.x 系列は最初から用意された枝」と解釈します。

	与えられたタグ名が、タグなのか枝タグなのかを判定する 一般的な手段はない。


- タグを打つルール
		枝タグを生やしたときに、根元にタグを打つ。
		別の枝や幹にマージ作業を行ったとき、取込元の枝に点タグを打つ。

	sticky tag

		作業ファイルとの関係
			作業領域にあるファイル毎に割り当てられています。 (作業領域のディレクトリに割り当てられている、のではない。)
			確認するには、cvs stat file の Sticky Tag: の 部分を見る。

		コミットとの関係
			枝タグにひっついているファイルは、その枝(の成長点)に対してだけ commitが可能。
			他の枝にcommitするには、いったん cvs update で他の枝に移る (ひっつけ直す)必要があります。

			点タグにひっついているファイルはcommitできません。
			作業領域内でいじくる分には自由ですが、commitで変更を登録することはできません。

		引っ付け方
			cvs checkout -rTAG でタグを明示して取り出した 作業領域(の各ファイル)は、TAG に引っ付く。
			cvs update -rTAG にて作業領域を 別のタグに移動させると、その新しい TAG に引っ付く。
			 (作業領域内ローカルの変更は保存されます)

		剥がし方

			幹(1.x)には明示的なタグがありません。 幹に属するファイルは「タグが剥がれて」います。
				実際には、幹1.xという枝にひっついている、と解釈します。 従ってこれを commit すると
				幹1.xが伸びます。 任意の枝にcommitできるわけではないことに注意！

			cvs update -A とすると、作業領域は幹の最先端に移動し、タグが全部剥がれます。
			単にタグが剥がれるのではなく、作業領域内で変更しなかったものは幹の最新版に置換されることに注意。
				論理的には update -rHEAD と update -A は同じ

		ひっつきタグの確認

			% cvs status file
			ひっつきタグは作業領域毎ではなくファイル毎なので、 作業領域のタグを確認する際は
			手頃なファイルを指定してcvs status で確認します。

			同じ作業領域内に違うタグにひっついたファイルが混ざっている状態も可能。
			作業領域丸ごとではなく、ファイルを指定して違うタグで checkout -r すればそういう状態になりますが、
			混乱必至なので普通は絶対やらない。
			(用途がないわけではない。 リリース版には含めたくないファイルとか)

### Keyword substitution / expansion

- keyword list

  - $Header$
  - $Id$
  - $Author$
  - $Date$
  - $Name$
    - Tag名を表示するする場合に使うキーワード
  - $Locker$
  - $Log$
  - $RCSfile$
  - $Revision$
  - $Source$
  - $State$

- 置換モード

+--------+--------------------------------+-------------------+
| option | description                    | 置換結果           |
+--------+--------------------------------+-------------------+
| -kkv   | キーワード文字と値を生成(default)  | $Revision: 5.7 $  |
| -kk    | キーワード文字だけを生成           | $Revision$        |
| -ko    | 何も変換しない（登録時の文字列のまま）| $Revision: 1.1 $  |
| -kb    | -koに加え改行コードの変換もしない（バイナリファイルを扱う場合に有効）| $Revision: 1.1 $  |
| -kv    | キーワードの値だけを生成         　| 5.7               |
+--------+--------------------------------+-------------------+

- example

  ```
  static char const rcsid[]="@(#) $Id$";
  ```

  - @(#) は SCCS というバージョン管理システムの what コマンドで検索可能
  - $キーワード: テキスト $ は、RCS の ident コマンドで表示可能

module一覧表示
 $ cd /tmp
 $ cvs co -l -d temp .
 $ cd temp
 $ cvs -n up -d


get specified version file(特定のバージョンの取り出し方法）
  $ cvs update -p -r 1.1 file1 >file1

make release tree
  $ cvs export -r Release-tag project_dir


ソースだけを取り出す方法
  （CVSディレクトリを作成しないでソースだけを取り出す。よって、チェックインは出来ない。）
  $ cvs export -D now <module_name>

## file

### fileの追加
  ```
  $ cp file .
  $ cvs add -m "message" file
  $ cvs ci -m "message" file
  ```

### delete file
  ```
  $ rm file
  $ cvs remove -f file
  $ cvs ci -m "message"
  ```

### binary fileの追加

  ```
  $ cp bin-file .
  $ cvs add -kb -m "add binary file" bin-file
  $ cvs ci -m "first checkin; binnary file" bin-file

  or

  $ cvs add -m "add binary file" bin-file
  $ cvs ci -m "first checkin; binnary file" bin-file
  $ cvs admin -kb bin-file <<<< ここでbinnary fileとして扱うように指定
  $ cvs update -A bin-file
  $ cvs commit -m "make it binary" bin-file
  ```

### branch
  ```
  $ cvs rtag -b -r release-1-0 release-1-0-patches tc
  $ cvs checkout -r release-1-0-patches tc
  $ cd tc
  $ vi file.c
  $ cvs ci -m "fix bug1"
  ```

### 通常の編集作業
  ```
  $ cvs up
  $ vi *.c
  $ cvs up

  $ cvs commit
    or
  $ cvs commit -m "message"
    or
  $ cvs commit -m "message" <fileA> <fileB>
  ```

### TAG付け
  ```
  $ cvs tag NAME
  ```

### for SSH
  ```
  $ export CVS_RSH=ssh
  $ export CVSROOT=:ext:<user>@16.161.40.195:/smcroot/cvs_smc

  $ cvs co <package>
  <user>@16.161.40.195's password: <password>
  ```

### setup pserver (for linux)
  ```
  $ more /etc/xinetd.d/cvspserver
  service cvspserver
  {
        socket_type = stream
        wait = no
        protocol = tcp
        user = root
        server = /usr/bin/cvs
        server_args =-f --allow-root=/usr/local/opensource/cvsrepo --allow-root=/tmp/xx pserver
        disable = no
  }

  $ kill -SIGHUP <pid_of_xinetd>
  ```

#### sample operation

```
$ cvs import -m 'initial message' tp vendor-tag release-tag

$ cd $trunk/
$ cvs co tp
$ cd tp

$ cvs status
   Working revision:	1.1.1.1	2014-02-09 08:01:56 +0900
   Repository revision:	1.1.1.1	/Users/hmizuno/Documents/lib/cvsrepo/tp/a,v
   Commit Identifier:	ol1C2ZK6SCWsQlox

$ cvs log
head: 1.1
branch: 1.1.1
symbolic names:
	release-tag: 1.1.1.1 <<<<<<<
	vendor-tag: 1.1.1 <<<<<<<
----------------------------
revision 1.1
branches:  1.1.1;
Initial revision
----------------------------
revision 1.1.1.1
initial message <<<<<<<<<

$ cvs tag trunk-1
$ vi a
$ cvs tag trunk-2
$ vi a
$ cvs tag trunk-3
$ cvs tab -b trunk-b1

$ cvs log
head: 1.3
symbolic names:
	trunk-b1: 1.3.0.2
	trunk-3: 1.3
	trunk-2: 1.2
	trunk-1: 1.1.1.1
	release-tag: 1.1.1.1
	vendor-tag: 1.1.1
total revisions: 4;	selected revisions: 4
----------------------------
revision 1.3
after trunk-2
----------------------------
revision 1.2
after trunk-1
----------------------------
revision 1.1
branches:  1.1.1;
Initial revision
----------------------------
revision 1.1.1.1
initial message

$ cd $branch/
$ cvs co -r trunk-b1 tp
$ cd tp
$ cvs status
   Working revision:	1.3	2014-02-09 08:13:02 +0900
   Repository revision:	1.3	/Users/hmizuno/Documents/lib/cvsrepo/tp/a,v
   Sticky Tag:		trunk-b1 (branch: 1.3.2)
   Sticky Date:		(none)
   Sticky Options:	(none)

$ cvs tag trunk-b1-1
$ cvs log
symbolic names:
	trunk-b1-1: 1.3
	trunk-b1: 1.3.0.2
	trunk-3: 1.3

$ vi a
$ cvs ci -m 'after trunk-b1-1 on trunk-b1'
$ cvs tag trunk-b1-2

$ cvs log
symbolic names:
	trunk-b1-2: 1.3.2.1
	trunk-b1-1: 1.3
	trunk-b1: 1.3.0.2
	trunk-3: 1.3
----------------------------
revision 1.3
branches:  1.3.2;
after trunk-2
----------------------------
revision 1.2
after trunk-1
----------------------------
revision 1.1
branches:  1.1.1;
Initial revision
----------------------------
revision 1.1.1.1
initial message
----------------------------
revision 1.3.2.1
after trunk-b1-1 on trunk-b1

$ vi a
$ cvs ci -m 'after trunk-b1-2 on trunk-b1'
$ cvs tag trunk-b1-3

$ cvs log
symbolic names:
	trunk-b1-3: 1.3.2.2
	trunk-b1-2: 1.3.2.1
	trunk-b1-1: 1.3
	trunk-b1: 1.3.0.2
	trunk-3: 1.3
----------------------------
revision 1.3
branches:  1.3.2;
after trunk-2
----------------------------
revision 1.2
after trunk-1
----------------------------
	:
----------------------------
revision 1.3.2.2
after trunk-b1-2 on trunk-b1
----------------------------
revision 1.3.2.1
after trunk-b1-1 on trunk-b1

$ cd $trunk/tp

$ cvs up
$ cat a
initial
edit after trunk-1 tag
edit after trunk-2 tag

$ cvs up -j trunk-b1
cvs update: Updating .
RCS file: /Users/hmizuno/Documents/lib/cvsrepo/tp/a,v
retrieving revision 1.3
retrieving revision 1.3.2.2
Merging differences between 1.3 and 1.3.2.2 into a

$ cat a
initial
edit after trunk-1 tag
edit after trunk-2 tag
edit after trunk-b1-1 on trunk-b1
edit after trunk-b1-2 on trunk-b1

$ cvs diff
===================================================================
retrieving revision 1.3
diff -r1.3 a
3a4,5
> edit after trunk-b1-1 on trunk-b1
> edit after trunk-b1-2 on trunk-b1

$ cvs ci -m 'after -j trunk-b1 ( trunk-b1-3 )'
$ cvs tag trunk-4

$ cvs log
head: 1.4
symbolic names:
	trunk-4: 1.4
	trunk-b1-3: 1.3.2.2
	trunk-b1-2: 1.3.2.1
	trunk-b1-1: 1.3
	trunk-b1: 1.3.0.2
	trunk-3: 1.3
	trunk-2: 1.2
	trunk-1: 1.1.1.1
	release-tag: 1.1.1.1
	vendor-tag: 1.1.1
----------------------------
revision 1.4
after -j trunk-b1 ( trunk-b1-3 )
----------------------------
revision 1.3
branches:  1.3.2;
after trunk-2
----------------------------
revision 1.2
after trunk-1
----------------------------

----------------------------
revision 1.3.2.2
after trunk-b1-2 on trunk-b1
----------------------------
revision 1.3.2.1
after trunk-b1-1 on trunk-b1
==============================

$ vi a
$ cvs ci -m 'after trunk-b1-3 on trunk-b1'
$ cvs tag trunk-b1-4
$ vi a
$ cvs ci -m 'after trunk-b1-4 on trunk-b1'
$ cvs tag trunk-b1-5

$ cvs log
symbolic names:
	trunk-b1-5: 1.3.2.4
	trunk-b1-4: 1.3.2.3
	trunk-4: 1.4
	trunk-b1-3: 1.3.2.2
	trunk-b1-2: 1.3.2.1
	trunk-b1-1: 1.3
	trunk-b1: 1.3.0.2
	trunk-3: 1.3
	trunk-2: 1.2
	trunk-1: 1.1.1.1
	release-tag: 1.1.1.1
	vendor-tag: 1.1.1
----------------------------
revision 1.4
after -j trunk-b1 ( trunk-b1-3 )
----------------------------
revision 1.3
branches:  1.3.2;
after trunk-2
----------------------------
revision 1.2
after trunk-1
----------------------------
revision 1.1
branches:  1.1.1;
Initial revision
----------------------------
revision 1.1.1.1
initial message
----------------------------
revision 1.3.2.4
after trunk-b1-4 on trunk-b1
----------------------------
revision 1.3.2.3
after trunk-b1-3 on trunk-b1
----------------------------
revision 1.3.2.2
after trunk-b1-2 on trunk-b1
----------------------------
revision 1.3.2.1
after trunk-b1-1 on trunk-b1
=============================================================================


$ cat a
initial
edit after trunk-1 tag
edit after trunk-2 tag
edit after trunk-b1-1 on trunk-b1
edit after trunk-b1-2 on trunk-b1

$ cvs up -j trunk-b1-4 -j trunk-b1-5
cvs update: Updating .
RCS file: /Users/hmizuno/Documents/lib/cvsrepo/tp/a,v
retrieving revision 1.3.2.3
retrieving revision 1.3.2.4
Merging differences between 1.3.2.3 and 1.3.2.4 into a
rcsmerge: warning: conflicts during merge

$ cat a
initial
edit after trunk-1 tag
edit after trunk-2 tag
edit after trunk-b1-1 on trunk-b1
edit after trunk-b1-2 on trunk-b1
<<<<<<< a
=======
edit after trunk-b1-3 on trunk-b1
edit after trunk-b1-4 on trunk-b1
>>>>>>> 1.3.2.4

$ vi a
$ cvs ci -m 'after -j trunk-b1 ( trunk-b1-5 )'
$ cvs tag trunk-5

$ cd $branch/tp

$ vi a
$ cvs ci -m 'after trunk-b1-5 on trunk-b1'
$ cvs tag trunk-b1-6
$ vi a
$ cvs ci -m 'after trunk-b1-5 on trunk-b1'
$ cvs tag trunk-b1-7

$ cd $trunk/tp

$ cvs up -j trunk-b1
cvs update: Updating .
RCS file: /Users/hmizuno/Documents/lib/cvsrepo/tp/a,v
retrieving revision 1.3
retrieving revision 1.3.2.6
Merging differences between 1.3 and 1.3.2.6 into a
rcsmerge: warning: conflicts during merge
HiroMacPro:tp hmizuno$ cat a
initial
edit after trunk-1 tag
edit after trunk-2 tag
<<<<<<< a
edit after trunk-b1-1 on trunk-b1
edit after trunk-b1-2 on trunk-b1
edit after trunk-b1-3 on trunk-b1
edit after trunk-b1-4 on trunk-b1
=======
edit after trunk-b1-1 on trunk-b1
edit after trunk-b1-2 on trunk-b1
edit after trunk-b1-3 on trunk-b1
edit after trunk-b1-4 on trunk-b1
edit after trunk-b1-5 on trunk-b1
edit after trunk-b1-6 on trunk-b1
>>>>>>> 1.3.2.6

$ cvs up -j trunk-b1-2 -j trunk-b1-3
cvs update: Updating .
C a

$ vi a
$ cvs ci -m 'after -j trunk-b1 ( trunk-b1-7 )'
$ cvs tag trunk-6

$ cvs log
symbolic names:
	trunk-6: 1.6
	trunk-b1-7: 1.3.2.6
	trunk-b1-6: 1.3.2.5
	trunk-5: 1.5
	trunk-b1-5: 1.3.2.4
	trunk-b1-4: 1.3.2.3
	trunk-4: 1.4
	trunk-b1-3: 1.3.2.2
	trunk-b1-2: 1.3.2.1
	trunk-b1-1: 1.3
	trunk-b1: 1.3.0.2
	trunk-3: 1.3
	trunk-2: 1.2
	trunk-1: 1.1.1.1
	release-tag: 1.1.1.1
	vendor-tag: 1.1.1
----------------------------
revision 1.6
after -j trunk-b1 ( trunk-b1-7 )
----------------------------
revision 1.5
after -j trunk-b1 ( trunk-b1-5 )
----------------------------
revision 1.4
after -j trunk-b1 ( trunk-b1-3 )
----------------------------
revision 1.3
branches:  1.3.2;
after trunk-2
----------------------------
revision 1.2
after trunk-1
----------------------------
revision 1.1
branches:  1.1.1;
Initial revision
----------------------------
revision 1.1.1.1
initial message
----------------------------
revision 1.3.2.6
after trunk-b1-5 on trunk-b1
----------------------------
revision 1.3.2.5
after trunk-b1-5 on trunk-b1
----------------------------
revision 1.3.2.4
after trunk-b1-4 on trunk-b1
----------------------------
revision 1.3.2.3
after trunk-b1-3 on trunk-b1
----------------------------
revision 1.3.2.2
after trunk-b1-2 on trunk-b1
----------------------------
revision 1.3.2.1
after trunk-b1-1 on trunk-b1
```
