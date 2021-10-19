# initial setup

```
#### system
yum check-update
yum install centos-release-scl
yum install epel-release

#	timedatectl list-timezone
#	timedatectl set-timezone Aasia/Tokyo

#### user
$ LANG=C xdg-user-dirs-gtk-update
```
### color setting

- ls --color={tty|auto|never}
- vi
  - syntax off / syntax on

# gnu global

- install

```
$ cat global-6.6.3.tar.gz |gzip -dc |tar xf -
$ cd global-6.6.3
$ ./configure CC=gcc CFLAGS=-std=gnu99
$ make
$ sudo make install
```

- initial ( make tagfiles)
  - cd $SRC_TOP_DIR
  - gtags [-v]

- operation
  - 参照箇所調査
    - global -rx <name> 
  - 定義箇所調査
    - global -dx <name>
  - シンボル調査
    - global -sx <name>
  - 文字列抽出（grep）
    - global -gx <name>
  - ファイルに定義されているシンボルを表示
    - global –f <file_name>

- config
  - cp /usr/local/share/gtags/gtags.conf $HOME/.globalrc
  - cp /usr/local/share/gtags/gtags.conf $PROJ_TOPDIR
  - vi $PROJ_TOPDIR gtags.conf

- emacs macro
  - 「gtags.el」
  - 任意のフォルダに global-x.x.x ディレクトリ内にある 「gtags.el」 をコピー
  - ※ 「.emacs」に以下の一文を追加
'''
$ cp gtags.el ~/lisp

$ vi .emacs
;; load-pathに追加
(setq load-path (cons "~/lisp" load-path))

;; gnu global
(require 'gtags)

(global-set-key "\M-t" 'gtags-find-tag)
(global-set-key "\M-r" 'gtags-find-rtag)
(global-set-key "\M-s" 'gtags-find-symbol)
(global-set-key "\C-t" 'gtags-pop-stack)

'''
  - Meadowを利用する場合は、C:\Meadow\site-lisp ディレクトリにgtags.elをコピー

- vi macro
  - 「gtags.vim」
  - 「$HOME/.vim/plugin」フォルダに global-x.x.x ディレクトリ内にある 「gtags.vim」 をコピー
'''
cp /usr/local/share/gtags/gtags.vim $HOME/.vim/plugin
'''

# gcc

- show all gcc internal defined macro

```
gcc -dM -E - < /dev/null

# sample operation :
# gcc -dM -E - < /dev/null |grep linux
```

- install basic version
```
yum install gcc
gcc --version
gcc (GCC) 4.8.5 20150623 (Red Hat 4.8.5-36)
```

- install newer version
```
yum install centos-release-scl # install SCL
yum search devtoolset # check version

yum install devtoolset-6-gcc
yum install devtoolset-6-gcc-c++
yum install devtoolset-7-gcc
yum install devtoolset-7-gcc-c++
yum install devtoolset-8-gcc
yum install devtoolset-8-gcc-c++

ls /opt/rh/
devtoolset-6  devtoolset-7  devtoolset-8
```

- change newer version
```
$ scl enable devtoolset-8 bash
$ gcc --version
gcc (GCC) 8.2.1 20180905 (Red Hat 8.2.1-3)

$ scl enable devtoolset-6 bash
$ gcc --version
gcc (GCC) 6.3.1 20170216 (Red Hat 6.3.1-3)

$ exit
$ exit
$ gcc --version
gcc (GCC) 4.8.5 20150623 (Red Hat 4.8.5-36)

```

# command

### sudo

```
# visudo

最後に以下の行を追加する。
foo         ALL=(ALL)       ALL

```

### audit

- log file
  - /var/log/audit/audit.log

- define rule
  - /etc/audit/audit.rules

- rule type
  - 「制御ルール」
  - 「ファイルシステムルール」
  - 「システムコールルール」

- file sytem rule
  - format : auditctl -w path_to_file -p permissions -k key_name
  - path_to_file : 監査対象のファイルもしくはディレクトリー
  - permissions : ログ記録されるパーミッション
    - r : 読み取りアクセス
    - w : 書き込みアクセス
    - x : 実行アクセス
    - a : 属性変更
  - key_name : どのルールまたはルールセットが特定のログエントリーを生成したかを特定する際に役立つオプションの文字列




```
$ show
auditctl -l		# list rule
auditctl -s		# print audit system status

$ delete
auditctl -D		# DELETE all audit rules
auditctl -D -k <key>	# DELETE audit rules of <key>
```

- sample operation
```
$ FILE
auditctl -w /home/foo/tmp -k home_foo_tmp # = ( -p rwxa )

auditctl -w /home/foo/tmp/foo -p r -k foo_r
$ log by "run test.sh ( foo/a.sh )"
type=SYSCALL msg=audit(1525138211.248:266442): arch=c000003e syscall=2 success=yes exit=3 a0=1233240 a1=0 a2=ffffffffffffff80 a3=7ffd0d36fb20 items=1 ppid=28186 pid=28187 auid=5604 uid=5604 gid=400 euid=5604 suid=5604 fsuid=5604 egid=400 sgid=400 fsgid=400 tty=pts1 ses=24651 comm="sh" exe="/usr/bin/bash" subj=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023 key="foo_r"
type=CWD msg=audit(1525138211.248:266442):  cwd="/home/hmizuno/tmp"
type=PATH msg=audit(1525138211.248:266442): item=0 name="foo/a.sh" inode=21923323 dev=08:02 mode=0100755 ouid=5604 ogid=400 rdev=00:00 obj=unconfined_u:object_r:user_tmp_t:s0 objtype=NORMAL
type=SYSCALL msg=audit(1525138211.248:266443): arch=c000003e syscall=2 success=yes exit=3 a0=122e790 a1=0 a2=8 a3=4 items=1 ppid=28186 pid=28187 auid=5604 uid=5604 gid=400 euid=5604 suid=5604 fsuid=5604 egid=400 sgid=400 fsgid=400 tty=pts1 ses=24651 comm="sh" exe="/usr/bin/bash" subj=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023 key="foo_r"
type=CWD msg=audit(1525138211.248:266443):  cwd="/home/hmizuno/tmp"
type=PATH msg=audit(1525138211.248:266443): item=0 name="foo/a.sh" inode=21923323 dev=08:02 mode=0100755 ouid=5604 ogid=400 rdev=00:00 obj=unconfined_u:object_r:user_tmp_t:s0 objtype=NORMAL

$ log by "run test.sh ( . ./foo/a.sh )"
type=SYSCALL msg=audit(1525138355.016:266444): arch=c000003e syscall=2 success=yes exit=3 a0=20cc2c0 a1=0 a2=435640 a3=fffffffffffff8c7 items=1 ppid=27936 pid=28225 auid=5604 uid=5604 gid=400 euid=5604 suid=5604 fsuid=5604 egid=400 sgid=400 fsgid=400 tty=pts1 ses=24651 comm="sh" exe="/usr/bin/bash" subj=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023 key="foo_r"
type=CWD msg=audit(1525138355.016:266444):  cwd="/home/hmizuno/tmp"
type=PATH msg=audit(1525138355.016:266444): item=0 name="./foo/a.sh" inode=21923323 dev=08:02 mode=0100755 ouid=5604 ogid=400 rdev=00:00 obj=unconfined_u:object_r:user_tmp_t:s0 objtype=NORMAL

$ auditctl -a exit,always -S open -F path=/usr/local/tmp/test.log -F success=1
$ auditctl -l
LIST_RULES: exit,always watch=/usr/local/tmp/test.log success=1 (0x1) syscall=open

$ cat /var/log/audit/audit.log
type=SYSCALL msg=audit(1373464694.690:74): arch=40000003 syscall=5 success=yes exit=3 a0=8048614 a1=0 a2=1b6 a3=804862d items=1 ppid=2044 pid=2162 auid=0 uid=0 gid=0 euid=0 suid=0 fsuid=0 egid=0 sgid=0 fsgid=0 tty=pts0 ses=2 comm="file" exe="/usr/local/tmp/c/file" key=(null)
type=CWD msg=audit(1373464694.690:74):  cwd="/usr/local/tmp/c"
type=PATH msg=audit(1373464694.690:74): item=0 name="/usr/local/tmp/test.log" inode=414336 dev=fd:00 mode=0100644 ouid=0 ogid=0 rdev=00:00
```

### grep

- extract word
```
$ cat foo |grep -o -E '\w+'
$ cat foo |grep -o -E '\w+' |sort -u
```

### rm

- remove / delete bad file with bad char code

```
$ ls
--bad_file_name

$ rm *
<< can NOT delete --bad_file_name

# you can delete it by the following command
$ rm ./-*
```

### diff

- create patch file
    ```
    diff -uprN > /tmp/patchfile
    ```
    - u : unified形式で出力
    - p : 変更箇所の関数名（C言語）を表示
    - r : サブディレクトリを再帰的に比較
    - N : 比較するファイルが存在しない場合、同名の空ファイルがあるものとして動作

- example : patchfile

```
diff -upNr ../org/a.c ./a.c
--- ../org/a.c	2018-02-16 08:21:20.612039797 +0900
+++ ./a.c	2018-02-16 08:21:50.492203648 +0900
@@ -1 +1,2 @@
 init
+add
diff -upNr ../org/dir/b.c ./dir/b.c
--- ../org/dir/b.c	2018-02-16 08:21:31.747100857 +0900
+++ ./dir/b.c	2018-02-16 08:22:00.190256828 +0900
@@ -1 +1,2 @@
 init
+add for c.c
```

### patch

  ```
  $ patch -p0 -d <topdir> </tmp/patchfile
  $ patch --strip=0 -d <topdir> </tmp/patchfile

  or

  $ cd <topdir>
  $ patch -p0 </tmp/patchfile
  $ patch --strip=0 </tmp/patchfile
  ```
  -p<num> | --strip <num>
	- default : num = 0
    - patchfileに記載されたpatch対象ファイルのパスを調整するパラメータ
	  Strip the smallest prefix containing <num> leading slashes('/') from the file path.
      For example, supposing the file name in the patch file was

             /u/howard/src/blurfl/blurfl.c

      -p0 gives : /u/howard/src/blurfl/blurfl.c : same !

      -p1 gives :  u/howard/src/blurfl/blurfl.c

      -p4 gives :               blurfl/blurfl.c
　　
	例：
	　ファイルパス： foo/bar/aaa/file.c

	　指定：　評価後のパス
	  -p0 -> foo/bar/aaa/file.c
	  -p1 -> bar/aaa/file.c
	  -p2 -> aaa/file.c

  -R | --reverse
	- un patch

# vi ( vim )

## color
- 設定ファイル
	- /usr/share/vim/vim74/colors/
- .vimrc
	- color desert
- vi command
	- :color desert

# locale

### LC_XXXX and LANG

| LC_XXXX	| desctiption		|
|---------------|---------------------|
|LC_ALL		|■LANGおよびこれ以外のLC_XXXより、最優先される設定■	|
|LC_MESSAGES	|メッセージに表示する言語、および肯定と否定の応答の設定	|
|LC_CTYPE	|文字の判定・操作・文字数のカウントなどの設定		|
|LC_MONETARY	|金額に関連する数値、通貨記号の表示の設定		|
|LC_NUMERIC	|金額に関係しない数値の表示（小数の区切り文字など）の設定|
|LC_TIME	|日付と時刻の表示の設定		|
|LC_COLLATE	|並び換えや正規表現に用いる文字の照合順序の設定	|
|LANG		|LC_XXXが設定されていないときに有効になる設定	|

### System locale

```
$ localectl status
$ localectl list-locales

$ localectl set-locale LANG=ja_JP.eucjp
$ cat /etc/locale.conf
LANG=ja_JP.eucjp

##### 設定を現在のコンソールに反映するには以下のコマンドを実行します
$ source /etc/locale.conf
```

# 文字コード

- localedef

  ```
  $ localedef -f <charmap> -i <sourcefile> <name>

  charmap : /usr/share/i18n/chrmaps/
	WINDOWS-31J
	SHIFT_JIS
  sourcefile : /usr/share/i18n/locales/ja_JP

  repository : /usr/lib/locale/locale-archive

  $ localedef --help
  Systems directory for character maps : /usr/share/i18n/charmaps
  repertoire maps: /usr/share/i18n/repertoiremaps
  locale path    : /usr/lib/locale:/usr/share/i18n
  ```

- SJIS対応(RHEL, CentOS)

  ```
  # localedef -f WINDOWS-31J -i ja_JP ja_JP.SJIS

  # locale -a | grep ja
  ja_JP
  ja_JP.eucjp
  ja_JP.sjis  <= 追加された。
  ja_JP.ujis
  ja_JP.utf8
  japanese
  japanese.euc
  ```

- delete ja_JP.XXXX

  ```
  # localedef --delete-from-archive ja_JP.sjis
  # locale -a |grep ja_JP
  ja_JP
  ja_JP.eucjp
  ja_JP.ujis
  ja_JP.utf8
  ```

- list localedef

  ```
  # localedef --list-archive |grep ja_JP
  ja_JP
  ja_JP.eucjp
  ja_JP.ujis
  ja_JP.utf8
  ```

- SJISファイルの扱い

```
$ vi $HOME/.exrc

set encoding=cp932
set fileencodings=utf8,cp932,latin

syntax off
set nohlsearch

```

# firewalld

- key: gw, network

- デフォルトの利用可能な事前定義サービスを一覧

	`# ls /usr/lib/firewalld/services/`

	- /usr/lib/firewalld/services/ にあるファイルは編集しないでください。

- システムもしくはユーザーが作成したサービスを一覧表示
	`# ls /etc/firewalld/services/`
  - /etc/firewalld/services/ にあるファイルのみを編集してください。

	サービスに対応する XML ファイルは /etc/firewalld/services/ にありません。
	サービスの追加または変更には、/usr/lib/firewalld/services/ のファイルを
	テンプレートとして使うことが可能

```
$ yum install firewalld
$ yum install firewall-config

$ systemctl start firewalld
$ systemctl stop firewalld
$ systemctl disable firewalld

$ systemctl status firewalld

$ firewall-cmd --state
```

- グラフィカルコンソールの起動：
`# firewall-config`

コマンドラインツールの利用：
$ firewall-cmd --version

$ firewall-cmd --help

コマンドを永続的にするには、すべてのコマンドに --permanent オプションを追加します。
（--direct コマンド (これらは元々一時的なもの) は除く）

コマンドを永続的にし、すぐに有効にするには、
	コマンドを --permanent を使用して 1 回、
	オプションなしで 1 回の合計 2 回入力する必要がある。

	ファイアーウォールのリロードは、上記のように単にコマンドを
	繰り返すよりも時間がかかる。

```
$ firewall-cmd --state
$ firewall-cmd --get-active-zones
$ firewall-cmd --zone=public --list-al
```

-  現在読み込まれているサービスを一覧表示
	/usr/lib/firewalld/services/ から読み込まれた事前定義サービスの名前と、
	現在読み込まれているカスタムサービス
	```
	# firewall-cmd --get-services
	```

/etc/firewalld/services/ で設定されたカスタムサービスを含めた
すべてのサービスが一覧表示
`# firewall-cmd --permanent --get-services`

■ファイアウォールのリロード
	# firewall-cmd --reload

■ユーザー接続を切断し、状態情報を破棄してファイアーウォールをリロード
	# firewall-cmd --complete-reload

■dmz で開いているすべてのポートを一覧表示
	# firewall-cmd --zone=dmz --list-ports


■workゾーンにsmtpサービスを追加して永続化する。
	# firewall-cmd --zone=work --add-service=smtp
	# firewall-cmd --zone=work --add-service=smtp --permanent

■workゾーンからsmtpサービスを削除して永続化する。
	# firewall-cmd --zone=work --remove-service=smtp
	# firewall-cmd --zone=work --remove-service=smtp --permanent

- 有効な設定を確認

	`firewall-cmd --list-services --zone=public  --permanent`

- 設定追加(sshとmysqlを追加)
	```
	firewall-cmd --add-service=ssh --zone=public --permanent
	firewall-cmd --add-service=mysql --zone=public --permanent
	```

- 設定削除(sshを削除)
	firewall-cmd --remove-service=ssh --zone=public --permanent

- 設定一覧を表示
	ls -lta /usr/lib/firewalld/services/

-  各設定毎の内容確認
	cat /usr/lib/firewalld/services/ssh.xml

- 有効な設定を確認
 	firewall-cmd --list-services --zone=public  --permanent     
	dhcpv6-client mysql ssh

# tips

## DESK top : folder

■　フォルダー名の言語を変更する方法：　change folder name language
　　$ LANG=C xdg-user-dirs-gtk-update

# logrotate
  /etc/logrotate.conf
	weekly		毎週ログを置き換える。毎日はdaily、毎月はmonthly

	rotate 4	ログを4世代分残す。
	       		weeky を指定した場合は4週間という意味になります。

	create		新規ログファイルをローテーションした直後に作成する

	compress	圧縮する。デフォルトでは圧縮しないようになっている.

	include /etc/logrotate.d
			各ログファイルの設定がおかれているパスを指定
			直接、logrotate.conf にローテーションの設定を書いても、
			あるいは、/etc/logrotate.d 以下にファイルを作成して
			そこにログローテート用の設定ファイルを置いても
			どちらでも構いません。
			RPM用のログローテーションのファイルは
			/etc/logrotate.d 以下に配置されています。

	/var/log/wtmp{	wtmp のログファイルは毎月1世代のみログを残し、
			所有者がroot でグループがutmp で且つ
			パーミッション値が664 のログファイルを作成するという意味。

  /etc/logrotate.d/<name>
	各ログファイルの設定は、/etc/logrotate.d 以下に各サービスごとに別ファイルに記述

  /var/lib/logrotate.log

  /var/lib/logrotate/logrotate.status
	ログローテート実施log

  /usr/bi/logrotate
	option
		-d .... debug
		-s .... status file name

# autotools

- autoconf
  - configure.ac（古いバージョンではconfigure.in）からconfigureを生成するプログラム
  - configure.ac
    - m4言語のマクロとシェルスクリプトで記載される。
- automake
  - Makefileを簡単に生成するための仕組み
  - Makefile.amからMakefile.inを生成する。
  - configureが、Makefile.inに環境固有の設定を追加してMakefileを生成する。

```
autoscan > configure.ac
            > autoheader  > config.h.in  V
                            Makefile.am  > automake > Makefile.in
            > aclocal     > aclocal.m4   > autoconf > configure
                                                        > config.status
                                                            > config.h
                                                            > Makefile
```  
# RHEL 7.X
     systemctl
     timedatectl
	timedatectl list-timezone
	timedatectl set-timezone <time-zone>
	timedatectl stauts

# SELinux
　config file: /etc/selinux/config
       #     enforcing - SELinux security policy is enforced.
       #     permissive - SELinux prints warnings instead of enforcing.
       #     disabled - No SELinux policy is loaded.
       SELINUX=enforcing
  基本操作：
	getenforce	モードの確認
	setenforce	モードの変更
	sestatus	SELinuxの状況の確認
	ps -Z	プロセスのセキュリティコンテキストの表示
	ls -Z	リソースのセキュリティコンテキストの表示
	getenforce	モードの確認
	getcon	セキュリティコンテキストの表示
	id	セキュリティコンテキストの表示
	newrole	ロールの変更

make for GCC
     # 必要なパッケージのインストール
     $ sudo yum install gmp-devel mpfr-devel libmpc-devel
     $ sudo yum install glibc-devel.i686

     # ビルド！
     $ mkdir -p ~/src
     $ cd ~/src
     $ curl -LO http://ftp.tsukuba.wide.ad.jp/software/gcc/releases/gcc-4.8.4/gcc-4.8.4.tar.gz
     $ tar fxz gcc-4.8.4.tar.gz
     $ cd gcc-4.8.4

     $ ./configure
     $ make


オブジェクトのシンボルを見る(symbol of object)
	objdumpコマンドの ?symsもしくは、--dynamic-syms
	nmコマンド

  # show symbols
	$ objdump -t object_file

  # show dynamic symbols
  $ objdump -T object_file

  # show all informations
  $ objdump -x object_file

get stderr info using pipe;
	commands > >(tee stdout.log) 2> >(tee stderr.log >&2)

modprove
	modprove -c .... print all information
	導入可能なモジュール一覧
		$ ls /lib/modules/`uname -r`

hdparm
	# df
	/dev/mapper/VolGroup00-LogVol00
	# hdparm -t /dev/mapper/VolGroup00-LogVol00 : test for reading disk data
	# hdparm -T /dev/mapper/VolGroup00-LogVol00 : test for reading cache data

paste
	行の連結
	１．1つのファイルの全部の行を連結する。
		$ cat infile | paste -d: -s -
	２．1つのファイルを3行ずつ連結する
		$ cat infile | paste -d: - - -
	３．複数のファイルを1行ずつ連結する
		$ paste -d: infile1 infile2

tr
  tr '\177-\377' '\040' |tr '\000-\002' '\040'
	制御コードをスペースに置き換えるコマンド

limits
  /etc/security/limits.d/90-nproc.conf
	*	soft	nproc	1024
	jboss	soft	nproc	unlimited
	root	soft	nproc	unlimited

unrm
 unrm (for ext2)
 http://freecode.com/projects/unrm

 extundelete (for ext3/ext4)
 http://extundelete.sourceforge.net/

lsof
  processがオープンしているファイルの数を調査する。
  /usr/sbin/lsof

ulimit
  /etc/security/limits.conf
   user_name soft nofile nnnnnn
   user_name hard nofile nnnnnn

keyboardレイアウト変更

  system-config-keyboardコマンド

  /etc/sysconfig/keyboard
    KEYTABLE="jp106"

	    usキーボード
		    KEYTABLE="us"
		    MODEL="pc105+inet"
		    LAYOUT="us"
		    KEYBOARDTYPE="pc"

    	jpキーボード
		    KEYTABLE="jp106"
		    MODEL="jp106"
		    LAYOUT="jp"
		    KEYBOARDTYPE="pc"

  /etc/X11/xorg.conf
    XkbModel jp106
    XkbLayout jp

	# JIS配列キーボードに変更
	setxkbmap -rules evdev -model jp106 -layout jp
	# US配列キーボードに変更
	setxkbmap -rules evdev -model us -layout us

fio
  file I/Oのベンチマークツール（フリーツール）
　http://freecode.com/projects/fio
  http://pkgs.repoforge.org/fio/


I/O card
  PCIバス
    - 32bit幅：データ転送量：33MHz / 133MB/s
    - 64bit幅：データ転送量：66MHz / 533MB/s
  PCI-Xバス
    - 64bit : 133MHz / 1.06GB/s
    - PCIのスロットにPCI-Xを挿せるし、その逆も可能
  PCI-X 2.0
    - 1クロックあたり2～4回のデータ転送をサポート
    - 133MHz / データ転送回数は266MHz(133MHzx2)あるいは533MHz(133MHzx4)動作相当
    - データ転送量：4.24GB/s(133MHzx4の場合)

  PCIe (PCI Express 1.1, 3GIO)
    - PCI busとの互換性はない。
    - 最小構成の伝送路(「レーン」と呼ばれる)は、片方向2.5Gbps、
      双方向5.0Gbpsの全二重通信が可能
    - 実効データ転送レートは片方向 2.0Gbps(250MB/s)、双方向4.0Gbps(500MB/s)
    - 実際のPCI Expressポートはこのレーンを複数束ねた構成
    - PCI Express x 1, x 2, x 4 ... x 32(8GB/s)

  PCIe 2.0 ( PCI Express 2.0 )
    - 片方向5.0Gbps、双方向10Gbpsに拡張
    - 実効データ転送レートは片方向 4Gbps(500MB/s)、双方向8Gbps(1GB/s)
    - 16レーンを用いた場合の転送速度は16GB/sとなる

InfiniBand
  InfiniBandの通信速度は1チャネルあたり片方向 2.5Gbps
  1本のケーブルに最高12チャネルを収納できる.
  ケーブル単位では最高で片方向30Gbps、双方向60Gbpsまでの対応が可能

  InfiniBand ソフトウェアは Remote Direct Memory Access (RDMA) をサポートしている。
　少ない CPU オーバーヘッドで高速かつ遅延 (メッセージが 2 ノード間を移動する時間) の
　少ないデータ転送を可能にしています。
　ただし、この RDMA の利点を生かすことができるのは、RDMA の機能を使えるように
　開発されたアプリケーションだけです。HP MPI では、この RDMA の機能を利用しています。

　InfiniBandネットワークに接続するサーバーでは、InfiniBandホストスタックソフトウェア（ドライバー）が
　動作していなければなりません。
　Mellanoxテクノロジーに基づくメザニンHCAの場合、HPは、
　Linux 64ビットオペレーティングシステム上ではMellanox OFEDおよびVoltaire OFEDドライバースタック、
　Microsoft Windows（HPC）Server 2008上ではMellanox WinOFドライバースタックをサポートします。

chkconfig
  $ chkconfig --list

start services...
  $ chkconfig vsftpd on
  $ service vsftpd start

# rpm

```
  install:
    $ rpm -i *.rpm
    $ rpm -i --force *.rpm

  update
    $ rpm -U *.rpm

  uninstall:
    $ rpm -e *.rpm

  get file list from *.rpm file
    $ rpm -qlp *.rpm

  get file list from system
    $ rpm -qla

  get package list from system
    $ rpm -qa

  get file list from installed package
    $ rpm -ql <package_name>

  get file list of package
    $ rpm -qlp *.rpm

  get package list which is installed
    $ rpm -qa

  get file list which is installded
    $ rpm -qla

  get package name which file depends. (OSにインストールされているファイルが属するパッケージを表示する)
    $ rpm -qf /usr/bin/sar
    sysstat-5.0.5-11.rhel4

  extract file : 解凍
    $ rpm2cpio mysql-toolkit-A.02.00-0.product.redhat.i386.rpm |cpio -idv

  パッケージの依存関係をチェックする
    rpm -ivh --test foo.rpm
```


# yum

- initialize

  ```
  yum check-update
  yum install centos-release-scl
  yum install epel-release
  ```

- tips

  - can NOT install manual
    - check /etc/yum.conf and commnt out
      ```
      # no manual flag
      # tsflags=nodocs
	  ```
    - overwrite this option in command line
	  ```
	  yum reinstall --setopt=tsflags='' -y <package_name>
	  yum install --setopt=tsflags='' -y <package_name>
	  ```

- install / uninstall / update

  ```
  yum install gcc
  yum reinstall gcc

  yum erase gcc = (yum remove gcc)

  yum check-update
  yum update <package_name>
  
  # update all package without kernel
  yum update --exclude=kernel*

  yum update-to		# Update one or all packages to a particular version
  yum upgrade		# Update packages taking obsoletes into account

  yum localinstall	# Install a package from a local file, http, or ftp
  yum localinstall XXXX.rpm
  yum localinstall http://xxxx

  ```

- display (list/search)

  ```
  yum list installed
  yum list available
  yum list all
  yum list kernel
  yum list

  yum search <package_name>
  yum updateinfo <package_name>

  yum deplist <package_name>  # dependencies for package

  yum provides <file_name>
  yum provides */clang-c/Index.h
  ```

- group

  ```
  yum grouplist
  yum groupinfo "web Server"
  ```

- troubleshoot / maintain / history

  ```
  cat /var/log/yum.log
  yum history
  yum history list
  yum history list <n>
  yum history info <n>
  yum history undo <n>
  yum history redo <n>

  yum clean <package>
  yum clean all
  ```

- repository

    ```
  　$ yum repolist
  　$ yum repolist all
    ```

- clean up

  ```
  rm -rf /var/cache/yum/*
  yum clean all
  ```
- download only

  ```
  # download all related package
  repotrack -p /tmp/download_dir llvm-toolset-7-clang

  # download package
  yum reinstall --downloadonly --downloaddir=/tmp/download_dir package_name
  ```

- check-update

  - 下記のようなインストールが出なくなるまで、複数回実行することで、ミラーサーバの情報がアップデートされる。

    ```
    # yum check-update
    　　読み込んだプラグイン:fastestmirror, langpacks
    　　base										 | 3.6 kB  00:00:00     
    　　extras                                                                   | 3.4 kB  00:00:00     
    　　updates                                                                  | 3.4 kB  00:00:00     
    　　updates/7/x86_64/primary_db                                              | 6.0 MB  00:00:01     
    ```  
## yum tips : error

- ```[Errno 256] No more mirrors to try.```

  - check yum repository directory : # ll /etc/yum.repos.d
  - delete old yum cache
    ```
    # rm -fr /var/cache/yum/*
    # yum clean all
    ```
  - check repository : # yum repolist
  
## yum config

- /etc/yum.conf

- proxy
	```
	/etc/yum.conf
		# Proxy Setting
		# proxy=http://your.proxy.server:8080
		proxy=http://autocache.hpecorp.net/ # hpe proxy
		# proxy_username=username
		# proxy_password=password
	```
- repository
	- 設定ファイル：

	```
	/etc/yum.repos.d/
		packagekit-media.repo
		redhat.repo
		rhel-source.repo
	```

  - ＩＳＯイメージをリポジトリとして利用

	```
　# vi /etc/yum.repos.d/dvd.repo

	[dvd]
	name=dvd
	baseurl='file:///media/RHEL-6.8 Workstation.x86_64'
	gpgcheck=1
	enabled=1
	gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release

	# yum search zsh

  # 複数のＤＶＤを指定可能
	# ls dve*repo
	dvd.repo	dvd2.repo

	# cat dvd2.repo
	[dvd2]
	name=dvd2
	baseurl='file:///media/Supp-6.8 RHEL-6 x86_64'
	gpgcheck=1
	enabled=1
	gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release

	# yum repolist
	repo id                name             status
	dvd                    dvd                4114
	dvd2                   dvd2                 32
	repolist: 4146
	```

## epel

- epel repository

```
$ sudo yum -y install epel-release
```

## SCL

- BASIC

  - SCL(Software Collections)
  - Red Hat が提供する最新アプリケーション（安定版）のCentOS版パッケージ
  - yumの旧パッケージと共存できる。
  - 2つのリポジトリパッケージが存在
    - centos-release-scl-rh : RHELSC互換の各種パッケージの提供
      - http://mirror.centos.org/centos/7/sclo/x86_64/rh/
    - centos-release-scl : CentOS SCLo SIGが提供するパッケージの提供
      - http://mirror.centos.org/centos/7/sclo/x86_64/sclo/

- SCL install

- リポジトリの追加
  - yumでリポジトリを追加します。/etc/yum.repos.d/CentOS-SCL.repo が追加されます。
  ```
  # yum install centos-release-scl

  ## OLD:2019/3/14 ## yum install centos-release-SCL
  ```

- パッケージのインストール
  - 通常のyumパッケージと SCL パッケージではインストールするパッケージ名が事なる
  ```
  yum install perl516
  ```

- SCL パッケージの有効化

- SCL パッケージではインストールされる場所は、/opt/rh 以下
  - 下記は、シェル(bash)を起動する方法
    ```
    $ scl enable perl516 bash
    $ bash # <<< SCLのbashが動作する
    ```

  - 直接実行する

    ```
    $ /opt/rh/perl516/root/usr/bin/perl -v
    ```

- SCL パッケージ内容

```
  $ yum list | grep centos.alt
```


# kdump
- 設定作業

```
  #ダンプ取得用のカーネル設定
  /etc/grub.confのkernel行 に”crashkernel=128M@16M”を追加
  #ダンプ取得サービスの初期起動設定
  chkconfig kdump on
  #ダンプ取得サービスの起動
  service kdump start
  #再起動
  reboot

  【確認作業】

  #擬似的にpanicを発生させる
  echo c > /proc/sysrq-trigger
    ⇒panicが発生し、/var/crash/xxxx/vmcoreファイル作成され、システムがリセットされる。

NFS config RHEL_7
    SERVER:
    # yum install nfs-utils
    # mkdir -p /exports/nfsdir
    # vi /etc/exports
    /exports/nfsdir client1,client2(rw)
    # systemctl start nfs
    # systemctl enable nfs
    # exportfs -v
    # firewall-cmd --add-service=nfs
    # firewall-cmd --add-service=nfs --permanent
    # firewall-cmd --reload
    # firewall-cmd --list-all

    CLIENT:
    # yum install nfs-utils
    # mkdir -p /nfsdir
    # mount -t nfs -o rw server_name:/exports/nfsdir /nfsdir
    # df
    # umount /nfsdir

    # vi /etc/auto.master.d/appdir.autofs
    /- /etc/auto.appdir

    # vi /etc/auto.appdir
    /nfsdir -fstype=nfs,rw,sync server_name:/exports/nfsdir

    # systemctl start autofs
    # systemctl enable autofs

    # df
    # ls /nfs/*
    # df

NFS tuning

  http://www.linux.or.jp/JF/JFdocs/NFS-HOWTO/performance.html

  /proc/net/rpc/nfsd
  /proc/fs/nfsd/threads

  5.6. NFSD のインスタンスの数

  Linux でも他の OS でも、ほとんどの起動スクリプトでは、 nfsd のインスタンスを 8 つ起動します。 NFS の最初の頃に Sun はこの値を経験則として決め、その後はみんなこの値をコピーしているのです。どのくらいのプロセス数が最適かを決める良い基準はありませんが、トラフィックの大きいサーバではより大きな値にするのが良いでしょう。最低でもプロセッサ当たりひとつのデーモンは起動すべきで、プロセッサ当たり 4 から 8 というのがだいたいの目安になるでしょう。 2.4 以降のカーネルを使っている人は、各 nfsd スレッドがどのくらい使われているかを /proc/net/rpc/nfsd で見てみるといいでしょう。このファイルの th 行の最後の 10 個の数字は、割り当て可能な最大値に対する各パーセンテージにそのスレッドがあった秒数を示しています。最初の 3 つの値が大きいときは、 nfsd のインスタンスを増やすほうが良いでしょう。これを行うには、nfsd を起動するときのコマンドラインオプションでインスタンスの数を与えます。 NFS の起動スクリプト (Red Hat なら /etc/rc.d/init.d/nfs) では RPCNFSDCOUNT で指定します。詳細は nfsd(8) の man ページを見てください。

  $ cat /proc/net/rpc/nfsd
  rc 4079 734520 26943711
  fh 98 27624726 0 40705 7282916
  io 2898628369 1650724275
  th 8 5199 14336.690 1543.220 1212.590 0.000 310.260 89.140 74.400 53.130 0.000 442.890
  ra 16 838699 141 27 31 7 19 11 20 9 5 108934
  net 27682311 27682311 0 0
  rpc 27682310 1 1 0 0
  proc2 18 767 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
  proc3 22 2226 22877994 5135 1076432 805992 29 947903 385416 155192 124 0 0 173095 0 302 19335 103867 905570 17859 1629 2 203441

  ###############
  thの行について
  ###############
  最初の 8 は、起動している nfsd の数
  10個ある小数点の数字は、nfsdの利用状況

  一番右端の数字は、442.890 となっておりますが、これは、nfsdの利用率が
  90-100%の状態は 442.890 秒間あったことを示しております。同じく右から
  2番目の数字は、nfsdの利用率が80-90%の状態は 0.0 秒間であったこを示し
  ております。
```

# traceroute

# VNC

```
  $ vncserver
```

- access : http://hoopoe.elk:5802/

# swap

- 推奨size
  ```
    RHEL4のドキュメント
      http://www.redhat.com/docs/manuals/enterprise/RHEL-4-Manual/x8664-multi-install-guide/s1-diskpartitioning.html
      2GBまではRAMの２倍
      3GBだったら、2x2+(3-2)x1=5GB
      16GBだったら、 2x2+(16-2)x1=18GB
  ```

- file system swap
  ```
    作成方法
      # dd if=/dev/zero of=/swap bs=1024 count=131072
      # mkswap /swap
      # swapon /swap
      # cat /proc/swaps
      # cat /proc/meminfo

      # swapoff /swap
  ```

# RC tools

- rh-profiler
  - cat /proc/meminfo とか、 top, ps, netstat などのデータをcronで回して、テキストファイルとして保存

  - ftp://linux.jpn.hp.com/pub/rh-profiler-0.1-5.noarch.rpm

    トラブルシュートの際に、「メモリーの空きが足りない」というのを確認する場合、　free で見られるメモリー全体の空きではなく、 /proc/meminfo でLowFree, HighFreeを区別して見られるという条件が大切になってくることがあります。

- strace
  system call のトレース

- prelink
  これはRHEL3から採用されたprelink という仕組みのせいで、binaryファイルに使用しているshared libraryの情報が事前に(pre)リンク(link)される事で、ファイルのELF header のサイズが大きくなります。
  ( static linkされたbinary file および、scriptなどのbinaryで無いファイルについては、prelinkの対象外です。）

  prelink 自体は、/etc/cron.daily/prelik というcron jobから毎朝起動されます。　そのため、OSインストール直後のprelinkがかかる前の状態のファイルサイズと、数日後のprelinkがかかった後のファイルサイズは違ってくる事があります。

  また、/etc/sysconfig/prelink という設定ファイルの先頭に、
   PRELINKING = yes  と書いてあるので、初期設定ではprelink が実行されます。　ここを"no"にすることで、次のprelinkが実行された際に、binaryファイルからprelink 情報が削除されて、サイズはインストール直後のものに戻ります。

- tuning
 
  - vmstat
    free
    buff ...... SWAP領域のために使用されているbufferメモリのサイズ
    cache ..... FILEのI/Oのために使用されるcacheメモリのサイズ

- sleep
  usleep(unsigned long usec) .... maicro secのスリープ関数

- Hyper thread
  OS稼動中にHTのstatus を見るには、以下のコマンドで行えます。
  $ hpasmcli
  > show HT

  また、kernel がuni-processorなのか、SMPなのか、
  kernel に  noht などのパラメータが渡されていないかどうかを確認して下さい。

- thread

  - Linux Threads
    スレッドを同一メモリ空間を共有する複数のプロセスとして実装されている。

    従って、ユーザが ps or top コマンドで実行プロセスを確認すると並列スレッドの実行モジュールであっても、複数のプロセスとして見えてしまう。また、管理スレッドを必要とするため、多数のスレッド操作においては、NPTL よりもオーバヘッドがある。多数のスレッドを扱う場合は、性能のスケーラビリティに影響する。

    Linuxthreads - POSIX 1003.1c kernel threads for Linux
    Copyright 1996, 1997 Xavier Leroy (Xavier.Leroy@inria.fr)

    USING LINUXTHREADS:

        gcc -D_REENTRANT ... -lpthread

  - NPTL=Native Posix Threads Library

    カーネルレベルでのスレッドをサポートし、同一プ
    ロセスのスレッドが同じプロセス ID を持つように
    なったため、本来のスレッドモデルのアクティビティ
    として見える。従って ps or top コマンドでは、
    並列スレッド・モジュールは一個のプロセスとして
    見える。このスレッドモデルは、管理スレッドを必
    要としないため、性能的なスケーラビリティも良い。
    メモリに代表される資源的にも、light-weight と
    なります。

- ulimit (default : /etc/login.conf ???)
  -n プロセスがオープン出来るファイルの最大数
  -c coreファイルのサイズ (or unlimited)

- kernel parameter

```
    $ vi /etc/sysctl.conf
  
  値の有効化：
    # /sbin/sysctl -p

  現在の値の表示：
    # /sbin/sysctl -a


#################################################
# sample tuning
# for 2.4.21-32.ELsmp

fs.file-max=65535
kernel.shmmax=1073741824
kernel.shm-use-bigpages=1
kernel.msgmni=1024
kernel.sem=1000 32000 32 512
net.ipv4.tcp_sack=0
net.ipv4.tcp_timestamps=0
net.ipv4.tcp_max_syn_backlog=8192
vm.bdflush=100 1200 128 512 15 5000 100 0 0
#################################################


net.ipv4.ip_local_port_range=1024 65000

```

- /etc/init.d, rc

```
  # /sbin/chkconfig --list nfs
  # /sbin/chkconfig <name> on
  # /sbin/chkconfig --level <level> <name> on
```

- show version
```
  $ cat /etc/redhat-release
  Red Hat Enterprise Linux ES release 2.1 (Panama)
```

- X
  - config file : /etc/X11/xorg.conf  ...... RedHat 4

- kernel conf
  - I/O queue
  ```
   elevator=XXX .... CFQ(default), Deadline, Noop, AS
     grob.conf内のkernel行の設定
     CFQ (Completely Fair Queueing)
       ... process毎に1つのI/Oキューを保持
     Deadline ..... データベースアプリケーションに向く
  ```

- Security Enhanced Linux (SELinux)
  パフォーマンスについて
    通常5%ぐらいの性能劣化(悪い場合は30%ぐらいの劣化)

- GFS (Global File System)
  クラスターワイドでファイルシステムにアクセス

## cpio

```
	cpio -it : list file path
	cpio -it \*.java : list file path with pattern.
	cpio -id \*.java : extract file with pattern.
```

## shared libraries
```
  $ vi /etc/ld.so.conf
       dynamic linker/loaderがライブラリをサーチするパスの指定

  $ ldconfig -v ......... dynamic run-time bindingの再設定
```

- tools
  wget .... http get command

- commands
  ldd <execute> ..... 共有ライブラリへの依存関係を表示する

- top

```
  h or ?  Help            
  Space   Update display
  ^L      Redraw the screen
  q       Quit            
  oO      Change order of displayed fields
  fF      Add and remove fields
  W       Write configuration file ~/.toprc
  n or #  Set the number of processes to show
  u       Show only a specific user
  k       Kill a task (with any signal)
  r       Renice a task
  s       Set the delay in seconds between updates
  Toggle:
    C:collapsed SMP CPU info
    S:cumulative mode           
    I:Irix/Solaris view (SMP)   
    H:threads               
    i:idle processes        
    c:command line          
    l:load average
    m:memory info
    t:summary info
  Sort by:
    A:age               M:resident memory usage
    N:pid               T:time (or cumulative time)
    P:CPU usage
```

/proc/cpuinfo
/proc/devices

/proc/<pid>
  cmdline
  cpu ....... cpu info
  cwd ....... current working directory
  environ ... 環境変数
  exe ....... 実行ファイル
  fd ........
  maps ...... memory map info.
  mem
  mounts
  root ...... root pointer of file system
  stat
  statm
  status .... process status(pid, ppid, VmSize, etc...)

/proc/sys/fs/file-max
/proc/sys/net/ipv4/ip_local_port_range


SHMMAX (/proc/sys/kernel/shmmax)
  Set this parameter to half the size of physical
  RAM available on your system. This value cannot
  exceed 4294967295.

  For large system global area (SGA) sizes greater
  than 1.8 GB, choose one of the following:

  Run the following command as the root user to
  mount the shared memory file system each time
  you reboot your system.

    # mount -t shm shmfs -o nr_blocks=8g/dev/shm

  Use the Automounter to run the shared memory
  file system command automatically as follows:

    Edit the /etc/fstab file as the root user to
    include the following line:

    shm /dev/shm shmfs nr_blocks=8g 0 0

install gcc package
  install following rpm package
  - libstdc++-devel-3.2.3-42.i386.rpm
  - gcc-3.2.3-42.i386.rpm

  - gcc-c++-3.2.3-42.i386.rpm
  - libgcc-ssa-3.5ssa-0.20030801.48.i386.rpm
  - gcc-ssa-3.5ssa-0.20030801.48.i386.rpm
  - libstdc++-ssa-3.5ssa-0.20030801.48.i386.rpm
  - libstdc++-ssa-devel-3.5ssa-0.20030801.48.i386.rpm
  - gcc-c++-ssa-3.5ssa-0.20030801.48.i386.rpm

lan tool
  $ mii-tool
    eth0: negotiated 100baseTx-FD flow-control, link ok
  $ ethtool eth0
    Settings for eth0:
        Supported ports: [ TP MII ]
        Supported link modes:   10baseT/Half 10baseT/Full
                                100baseT/Half 100baseT/Full
        Supports auto-negotiation: Yes
        Advertised link modes:  10baseT/Half 10baseT/Full
                                100baseT/Half 100baseT/Full
        Advertised auto-negotiation: Yes
        Speed: 100Mb/s
        Duplex: Full
        Port: MII
        PHYAD: 1
        Transceiver: internal
        Auto-negotiation: on
        Current message level: 0x000020c1 (8385)
        Link detected: yes

lan
  [質問] tg3 driver をLinux上で使っている場合に、HUB/SWITCHとの
         negotiation speed などをどのように固定で指定しますか?

  [環境] Red Hat Linux

  [回答]
   tg3 driverには、オプションとしてspeedやduplexを渡す事が出来ません｡
   そのため、ドライバーロード後にethtoolコマンドを使い設定を行ないます｡
   /etc/modules.conf の中に post-install というオプションを指定する事
   により、この手順を自動化することが出来ます｡

     alias eth0 tg3
     post-install tg3 /usr/sbin/ethtool -s eth0 speed 1000 duplex full

  指定できるパラメータの例
　　speed 10 | 100 | 1000
    duplex half | full
　　autoneg on | off

mount problem
  server side  
    /etc/exports
    /etc/hosts.allow
    /etc/hosts.deny

  client side  

  both side
    - /etc/rc.d/rcX.d/にてnfsが起動するように変更
    - /etc/rc.d/rcX.d/にてipchainsとiptablesが
      起動しないように変更する。

      変更方法
        # /usr/sbin/ntsysv
          ....... /etc/rc.d/rcX.d/S/KXXXX の設定
        # /sbin/chkconfig --list nfs

ERROR: Cannot load /etc/httpd/modules/mod_jk.so into server:
  /lib/i686/libc.so.6: version `GLIBC_2.3' not found
  (required by /etc/httpd/modules/mod_jk.so)

  libcのバージョンの確認
  # /sbin/ldconfig -V
  ldconfig (GNU libc) 2.2.4

  $ rpm -qa |grep glibc
  glibc-2.2.4-32.18
  glibc-common-2.2.4-32.18
  glibc-devel-2.2.4-32.18

  glibcのアップデートを実行!

X windows
  # vi /etc/X11/XF86Config

  # If you'd like to switch the positions of your capslock and
  # control keys, use:
  #       Option  "XkbOptions"    "ctrl:swapcaps"
  # Or if you just want both to be control, use:
          Option  "XkbOptions"    "ctrl:nocaps"

  /usr/bin/setxkbmap -option "ctrl:nocaps"

mount command for HP-UX application CD :
      mount -o ro,map=off /dev/cdrom /mnt/cdrom
      mount -t iso9660 -o ro,map=off /dev/cdrom /mnt/cdrom
      /etc/fstab
        /dev/cdrom /mnt/cdrom iso9660 map=off,ro 0 0

### ssh

- sshポートフォワーディング

```
ssh -fNL 8081:target:tgt_port remote

-L : Local fowarding
-N : no commandline
-f : background

local:8081 port へのアクセスを、remoteへのSSL接続をつかって
remoteからtargetホストのtgt_portへ転送する。

　　ssh -fNL 8081:192.168.2.173:80 16.171.21.146
    #　targetの80ポートにアクセスする

```

```
#--------------------------------------
# ssh keyの作成
#
$ ssh-keygen -t rsa -C "your_email@example.com"
Enter file in which to save the key (/home/hmizuno/.ssh/id_rsa): <RETURN>
Enter passphrase (empty for no passphrase): <RETURN>
Enter same passphrase again: <RETURN>

#--------------------------------------
# パスワードなしでログインできるように設定
#
$ ssh-copy-id -o StrictHostKeyChecking=no -i $HOME/.ssh/id_rsa.pub <target_host>

#
#　上記コマンドが無い場合は、以下を実施
#
$ scp .ssh/id_rsa.pub <remote>:PUBKEY
$ ssh <remote>

# remoteでの作業

$ ls -l .ssh
-rw-------.  1 hmizuno assess  805  2月 14 10:11 authorized_keys
-rw-------.  1 hmizuno assess 1675  1月  2 19:20 id_rsa
-rw-r--r--.  1 hmizuno assess  406  1月  2 19:20 id_rsa.pub
-rw-r--r--.  1 hmizuno assess 1318  1月 30 10:37 known_hosts

# .ssh/authorized_keysにPUBKEYの内容を追加する。

$ vi .ssh/authorized_keys
ssh-rsa XXXX
ssh-rsa YYYY
:r PUBKEY



# 公開鍵を他のサイトに設定するためにキーをコピペバッファに読み込む
$ clip < ~/.ssh/id_rsa.pub

# 設定のチェック
$ ssh -T git@github.com


```

  How to auto connect (linux:ssh --> HP-UX)
  [linux]
    $ ssh-keygen -t dsa
    Generating public/private dsa key pair.
    Enter file in which to save the key (/home/shu/.ssh/id_dsa):
    Enter passphrase (empty for no passphrase):
    Enter same passphrase again:
    Your identification has been saved in /home/shu/.ssh/id_dsa.
    Your public key has been saved in /home/shu/.ssh/id_dsa.pub.
    The key fingerprint is:

    $ scp .ssh/id_dsa.pub hpux_host:pubkey

  [HP-UX]
    $ cat pubkey >> .ssh/authorized_keys
    $ chmod 600 .ssh/authorized_keys

  [linux]
  開始処理
    $ eval `ssh-agent`
    Agent pid 7589
      ..... ここで、環境変数(SSH_AUTH_SOCK、SSH_AGENT_PID)が設定される。

    $ ssh-add
    Enter passphrase for /home/shu/id_dsa:
    Identity added: /home/shu/id_dsa (/home/hmizuno/.ssh/id_dsa)

  実行
    $ ssh hpux-host ls
    成功!!!!

  終了処理
    $ ssh-agent -k
