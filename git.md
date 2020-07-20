# document

- https://git-scm.com/book/ja/v2

### alias
```
git config --global ailas.co 'checkout'
git config --global ailas.ci 'commit'
git config --global ailas.di 'diff'
git config --global ailas.br 'branch'
```

## git tar packing
```
git archive  --format=tar -o ~/linuxtools.dump.tar  HEAD
```

## push

- nothing	引数でブランチ指定がなければエラー
- current	現在チェックアウトしているブランチと同名のブランチを更新
- upstream	上流ブランチに設定されているブランチにPushする
- simple	(default)
  - 上流レポジトリへのpushであれば「upstream」と同じ動作
  - もしブランチ名が異なる場合はpushを中止
  - もし別のリモートレポジトリへの push であれば「current」と同じ動作。
- matching	ローカルのすべてのブランチを、リモートの同名のブランチに push する。

## reflog

- cancel / reset oprations

```
# check
git reflog

# reset to old commit point
git reset --hard <old_commit_point>
```

- sample operation

```
git reflog -n 5
  74f21a7 HEAD@{0}: reset: moving to HEAD^^  # This operation is BAD !!!! <<< need to calcel this.
  64606a0 HEAD@{1}: commit: third commit
  6be52a3 HEAD@{2}: commit: second commit
  74f21a7 HEAD@{3}: commit: first commit
  30fa85e HEAD@{4}: checkout: moving from master to test-branch
  
# cancel last reset command
git reset --hard "HEAD@{1}"
  HEAD is now at 64606a0 third commit
```

## checkout : switch : restore

- create new branch
  - git checkout -b <branch_name>
  - git switch -c <branch_name> : (-c : create)

- restore files
  - git checkout <file_name>
  - git restore <file_name>

## add

- git add -A : 追加されたファイルも含め全部のファイルをaddする。
- git add -u : バージョン管理しているファイルだけaddする。
- git add file_name : 指定したファイルだけaddする。

- git add -p
  -	修正したソースの一部分だけ選択してStagingする。
  - sub command
    - y	yes : stage this hunk
    - n	do not stage this hunk
    - s	split the current hunk into smaller hunks
    - e	manulally edit the current hunk

  - hunk … Stagingする対象のソースコードの一部分				

  - how to edit
    - 行の削除をキャンセルしたい場合は、行頭の"-"をスペース" "に変更する。(-を削除すると正しく動作しない。）			
    - 行の追加をキャンセルしたい場合は、"+"が付いている行を削除する。			
    - 修正しないと、削除と追加がそのまま行われる。			

  - sample
```
    -OPTA=option_a_string		
    +OPTB=option_b_string		
```
    - 削除のキャンセル：		
      - 変更前	-OPTA=option_a_string
      - 変更後	 OPTA=option_a_string
    - 削除のキャンセル：		
      - 変更前	+OPTB=option_b_string
      - 変更後	行を削除
 
## rebase

```
# basic operation
git checkout feature
git rebase develop

```
```
#
# change message
#
$ git log --oneline
0c59669 (HEAD -> test) F
e5b9982 E
d9cefd9 D
ea9b7f5 C
11cbb6b B
76fe793 A

$ git rebase -i 76fe793
>>>>> EDITOR:
pick 11cbb6b B
pick ea9b7f5 C
r d9cefd9 D
pick e5b9982 E
pick 0c59669 F
:wq

>>>>> EDITOR:
D new commit message
:wq

$ git log --oneline
fa95662 (HEAD -> test) F
6c7e665 E
5e4d2ec D new commit message
ea9b7f5 C
11cbb6b B
76fe793 A

```
```
#
#  squash commit (combine)
#
$ git rebase -i 76fe793
>>>>> EDITOR:
pick 11cbb6b B
pick 11cbb6b B
pick ea9b7f5 C
s 5e4d2ec D new commit message
pick 6c7e665 E
pick fa95662 F
:wq
>>>>> EDITOR:
# This is the 1st commit message:
C

# This is the commit message #2:
D new commit message

>>>>> EDITOR: EDIT !!!!
# This is the 1st commit message:
C and D new commit message

# This is the commit message #2:
D new commit message

$ glog |head
* 1428a3e (HEAD -> test) F
* 55b026d E
* 22dbf01 C and D new commit message
* 11cbb6b B
* 76fe793 A

```
```
#
#  split commit
#
$ git rebase -i 76fe793
>>>>> EDITOR:
pick 11cbb6b B
e 22dbf01 C and D new commit message
pick 55b026d E
pick 1428a3e F
:wq

$ git reset HEAD~
Unstaged changes after reset:
M       file

hmizuno@DODNZDRLGH MSYS ~/git/gitstudy
$ git diff
diff --git a/file b/file
index 223b783..9f7c5d3 100644
--- a/file
+++ b/file
@@ -1 +1,3 @@
 B
+C
+D

$ git add first_commit_file
$ git commit -m first_commit_file

$ git add second_commit_file
$ git commit -m second_commit_file

$ git rebase --continue
Successfully rebased and updated refs/heads/test.

$ glog |head
* 35f9687 (HEAD -> test) F
* b16edba E
* 5fbed97 add D line
* e00572e add C line
* 11cbb6b B
* 76fe793 A
```

```
#============
# signoff
#============

git remote add <new_remote> URL

# URL : 
#	remote : foo@host:path
#   local  : /foo/var/path

git fetch <new_remote>
git checkout <new_remote>/<feature/branch_name>

# start rebase
git rebase -i develop

# check changes  <<< REPEAT
git show

# OK : commit
git commit -s --amend

# NG : change and commit
git checkout HEAD~ -- <file> <file> ...
vi file # fix files
git add -- <file>
git commit -m "message"

# continue
git rebase --continue

# you can abort rebase
## ABORT : git rebase --abort

#
# repeat from >>> "check changes"
#

# push
git push -f origin <feature/branch_name>
```

```
##
## rebase -i commands.
##
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup <commit> = like "squash", but discard this commit's log message
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
# .       create a merge commit using the original merge commit's
# .       message (or the oneline, if no original merge commit was
# .       specified). Use -c <commit> to reword the commit message.
```

## remote

- add rmote over ssh

```
git remote add <name1> uid@hostname:<path>
git fetch <name1>
git checkout <branch_name>
```

  - tips:

```
git fetch <remote_name>

ERROR: "sh: git-upload-pack:  not found."

1. <<REMOTE>> : search absolute path git-upload-pack command on remote host

  $ login remote
  $ which git-upload-pack
  /usr/local/bin/git-upload-pack

2. <<LOCAL>> : setup remote config

git config remote.<remote_name>.uploadpack /usr/local/bin/git-upload-pack
git config remote.<remote_name>.receivepack /usr/local/bin/git-receive-pack

  ex : git config remote.origin.uploadpack /path/to/git-upload-pack
```

  - path

    - ~foo/path
    - /git/path

- add local

```
git remote add other_repo /absolute_path/top_dir_of_git_repo
git fetch other_repo
git branch -a
	remotes/otehr_repo/master
	remotes/otehr_repo/develop
	...
```

- push local repository to remote

```
git remote add origin git@github.com:hiromzn/foo.git
git push --all 
```

```
git remote add newrepo git@github.com:hiromzn/new_repository.git
git push --all newrepo
```

  - push specivic branch

```
git checkout <branch_name>
git push -u origin <branch_name>
```

## commit

```
# COMMT
git commit -m "commit message"

#----------------
# REMOVE
#----------------
# remove last one
git reset --hard HEAD~
# apply to remote
git push -f

#---------------------------
# change commit message
#---------------------------
# change latest one.
$ git commit --amend -m "replace message"

# change old message.
$ git rebase -i HEAD~3
--- VI ---
pick 348fc23 fix : support source (*.c) include definition
edit 4742b03 REV_2_2
pick ebd29be add : replace_hlist.sh
--- wq ---
$ git commit --amend -m "replace message"
$ git rebase --continue
```
## list / show

```
# show git objects
git show <any_objects>
git show 65a0887
git show develop
```

```
# show commit log
git log
git log --all                         # with all branch
git log --oneline                     # one line view
git log --oneline --decorate          # print ref name (branch pointer)
git log --oneline --decorate --graph  # show graph view
git log --date=format:'%Y-%m%d-%H%M:%S'
git log --date=format:'%Y-%m%d-%H%M-%S' --pretty=format:'%h %ad%d %s'
git log --date=format:'%Y-%m%d-%H%M-%S' --pretty=format:'%h %ad%d %<(30,
trunc)%s' # truncate message of right
git log --date=format:'%Y-%m%d-%H%M-%S' --pretty=format:'%h %ad%d %<(30,
trunc)%s' # truncate message of middle part
git log --date=format:'%Y-%m%d-%H%M-%S' --pretty=format:'%C(cyan)%h%Creset %C(black bold)%ad%Creset%C(auto)%d %s'

# show file commit log
git log <file_name>                   # commit log for <file_name>
git log -p <file_name>                #   with diff info.
git log -p --oneline -U0 <file_name>

# show file contents
git show <commitID>:<file_path>

# show file contents log
git blame <file_name>                 # file contents with last commit info.
git blame -L 5,8 <file_name>          #   with specify line range

# show config
git config -l          .... list all configrations
git config -l --local  .... list all local configrations
git config -l --global .... list all global configrations

# show tag
git tag
git tag -n[<n>]  ..... print tag message with <n> lines

# show branch
git branch
git branch -r
git branch --contains HEAD  # show current branch only
git branch -a
```

## checkout

- chenge branch / commit

  - git checkout <branch_name>
  - git checkout <commit_ID>

- ファイルの復活・復元

```
# 全ファイルを元に戻す（内容の復元、削除の取り消し）
git checkout -f
git checkout .

# 特定のファイルを元に戻す
git checkout <file_name>

# revert file to specify commit 
git checkout COMMIT_ID -- file_path
git checkout HEAD -- file_path
git checkout HEAD~ -- file_path

# revert file to specify commit with selecting patch
git checkout -p COMMIT_ID -- file_path

# selecting opration can use the following.
y - discard this hunk from index and worktree
n - do not discard this hunk from index and worktree
q - quit; do not discard this hunk or any of the remaining ones
a - discard this hunk and all later hunks in the file
d - do not discard this hunk or any of the later hunks in the file
g - select a hunk to go to
/ - search for a hunk matching the given regex
j - leave this hunk undecided, see next undecided hunk
J - leave this hunk undecided, see next hunk
e - manually edit the current hunk
? - print help

sample operation :
$ git checkout -p HEAD -- foo.c
diff --git a/foo.c b/foo.c
--- a/foo.c
+++ b/foo.c
        int             xxx;
+       int             foo;
        int             real;
Discard this hunk from index and worktree [y,n,q,a,d,j,J,g,/,e,?]? ?
y - discard this hunk from index and worktree
n - do not discard this hunk from index and worktree
q - quit; do not discard this hunk or any of the remaining ones
a - discard this hunk and all later hunks in the file
d - do not discard this hunk or any of the later hunks in the file
g - select a hunk to go to
/ - search for a hunk matching the given regex
j - leave this hunk undecided, see next undecided hunk
J - leave this hunk undecided, see next hunk
e - manually edit the current hunk
? - print help
diff --git a/foo.c b/foo.c
--- a/foo.c
+++ b/foo.c
        int             xxx;
+       int             foo;
        int             real;
Discard this hunk from index and worktree [y,n,q,a,d,j,J,g,/,e,?]?
```

## tags

```
git ls-remote --tags
git push --tags
git pull --tags
```

## delete

- delete commit & push

```
# delete local commit :  直前のコミットを破棄してファイルも戻す。
git reset --hard HEAD^
# delete remote master : localの内容を強制的にリモートpushする。
git push -f origin master
```

- delete untracked file
```
git clean -n -f -d     # check deleted untracked files only (does NOT delete it)
git clean -f -d        # delete untracked files
```

- git reset [option] position
  - behavior
    - move HEAD position to old.
    - change working tree or Index.
  - position
    - HEAD   : current HEAD point
    - HEAD~  : previous HEAD point
    - HEAD~2 : Two previous HEAD point
  - option
    - --soft  : no any change (working_tree and Index)
    - --mixed : (default) change Index only.
    - --hard  : change all (working_tree and Index)

## alias

```
// set alias
git config --local alias.current-branch 'rev-parse --abbrev-ref HEAD'

/// use alias
git current-branch
```
#### branch

```
# CREATE
git branch [branch_name]
git branch [branch_name] <commit_point>

# GET remote branch
git pull

# GET all remote branch to local
#!/bin/bash
for branch in $(git branch --all | grep '^\s*remotes' | egrep --invert-match '(:?HEAD|master)$'); do
    git branch --track "${branch##*/}" "$branch"
done
git fetch --all
git pull --all

# DELETE : local
git branch -d [branch_name]  	//ローカルブランチの削除
git branch -D [branch_name]  	//ローカルブランチの削除：マージされていないブランチを削除する場合

# DELETE : remote
git push --delete origin <branch_name> // リモートのブランチを削除
git push origin :<branch_name>

# RENAME
git branch -m [branch_name]  	//現在のブランチ名の変更

# CHANGE / CHECKOUT
git checkout [branch_name]  	//ブランチの移動/切り替え
git checkout -b <branch_name>	//branchを作成して、作成したブランチにに切り替える。
git checkout -b branch_name origin/branch_name //リモートブランチからチェックアウト

#
# PULL ALL remote branch
# bash 
for branch in $(git branch --all | grep '^\s*remotes' | egrep --invert-match '(:?HEAD|master)$'); do
    git branch --track "${branch##*/}" "$branch"
done
git fetch --all
git pull --all

// show or list
git branch    			// ローカルブランチの一覧
git branch -a 			//リモートとローカルのブランチの一覧
git branch -r			//リモートブランチの一覧
git rev-parse --abbrev-ref HEAD	// show current branch name

$ git branch -a -vv .... デフォルトのremote (upstream) branch名も表示する。
* fix1                  eaccfe8 [origin/fix1] fix
  master                3d16af3 [origin/master] new
  remotes/origin/HEAD   -> origin/master
  remotes/origin/fix1   eaccfe8 fix
  remotes/origin/master 3d16af3 new


$ git push --set-upstream origin <branch_name> ..... デフォルトのremote (upstream) branchを指定してPUSHする。

```

## access protocol

- https
  - git remote set-url origin https://<host_name>/hiromichi-mizuno/gccmsgana.git
  - ## need PASSWORD when you access remote
  
- https with TOKEN
  - git remote set-url origin https://<token-string>@<host_name>/<user_name>/<repo_name>.git
  - ## NO need PASSWORD

- ssh
  - git remote set-url origin ssh://<host_name>/<user_name>/<repo_name>.git
  - ## NO need PASSWORD ( when you setup ssh key config )

- git
  - git remote set-url origin git@<host_name>:<user_name>/<repository_name>.git
  - ## NO need PASSWORD

## change remote repositoy

```
git remote set-url origin http://zzzzz/wwww.git
git remote set-url origin git@github.com:hiromzn/www

git remote rename from_name to_name
```
  
## github.hpe.com

- token
  - https://github.hpe.com/hiromichi-mizuno
    - develop : a5521fadd292e405ed38774f84b9b0d968b58f0a

- access http using token

  - create token

    - https://github.hpe.com/settings/tokens
    - push [Generate new token]
    - token description [ <set_your_token_name> ]
    - check required functions [x]
    - push [Generate token]
    - push [copy token]
    - show message : "Make sure to copy your new personal access token now. You won’t be able to see it again!"
    - paste new token into secure place ! [ <token_string> ]
  
  - access http using token

  ```
  git remote set-url origin https://<token-string>@github.hpe.com/hiromichi-mizuno/gccmsgana.git
  git remote set-url origin https://hiromichi-mizuno:<token-string>@github.hpe.com/hiromichi-mizuno/gccmsgana.git
  ```

- clone
```
$ git remote set-url origin git@github.hpe.com:hiromichi-mizuno/<repository_name>.git
```

## setup

### ssh setup

#### ssh key

- Generating a new SSH key
	<https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/#generating-a-new-ssh-key>

  ```
  $ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
  Enter a file in which to save the key (/Users/you/.ssh/id_rsa): [Press enter]
  Enter passphrase (empty for no passphrase): [Type a passphrase] <<< [[PUSH RETURN only!!!]]
  Enter same passphrase again: [Type passphrase again]
  ```
  - OPTION: copy pubkey to remote server to login without password.

    ```
    $ ssh-copy-id -i $HOME/.ssh/id_rsa.pub [user_name@]hostname
    ```
  
- Adding a new SSH key to your GitHub account
	<https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/>
  ```
  $ clip < ~/.ssh/id_rsa.pub
  # Copies the contents of the id_rsa.pub file to your clipboard
  ```

- access check

  ```
  $ ssh -T git@github.com

  The authenticity of host 'github.hpe.com (15.115.198.33)' can't be established.
  ECDSA key fingerprint is SHA256:dIMDIEzOHy/5IaTOGRoR2Kzk/Uo6HOT0Wl0wtmT+zn0.
  Are you sure you want to continue connecting (yes/no)? yes
  Warning: Permanently added 'github.hpe.com,15.115.198.33' (ECDSA) to the list of known hosts.
  Hi hiromichi-mizuno! You've successfully authenticated, but GitHub does not provide shell access.
  ```

#### create empty マスターリポジトリ

- create master repository (with no file)

  - ex: https://github.hpe.com/hiromichi-mizuno/assesstest01.git

#### push to マスターリポジトリ

  ```
  $ git remote add origin https://github.hpe.com/hiromichi-mizuno/assesstest01.git
  $ git remote -v

  $ git push -u origin master
  ```

#### commandとの対応

- origin -- git clone --> origin/master & master
- origin -- git fetch --> origin/master
-        <-- git push --
- master <-- git merge --> origin/master

- git remote -v
  - origin  https://.../workshop (fetch)
  - origin  https://.../workshop (push)

#### config

```
git config --global merge.tool vimdiff
git config --global mergetool.prompt false
git config --global diff.tool vimdiff
git config --global difftool.prompt false
```

```
$ git config -l          .... list all configrations
$ git config -l --local  .... list all local configrations
$ git config -l --global .... list all global configrations

# configure for proxy
$ git config --global http.proxy http://proxy.jpn.hp.com:8080
$ git config --global https.proxy http://proxy.jpn.hp.com:8080

$ git remote -v
origin  https://github.com/hiromzn/study1 (fetch)
origin  https://github.com/hiromzn/study1 (push)

$ git remote set-url origin git@github.com:hiromzn/study1

$ git remote -v
origin  git@github.com:hiromzn/study1 (fetch)
origin  git@github.com:hiromzn/study1 (push)
```

#### merge

```
$ git merge iss53

# merge only one commit
$ git cherry-pick <commit>

# merge only some commit (from next comit of commit-1 to commit-2)
$ git cherry-pick <commit-1>..<commit-2>
```

#### rebase

```
git checkout fix
git rebase master
vi confilicts_files
git add confilicts_files
git rebase --continue  # go rebase
git rebase --abort     # cancel rebase
```

#### diff
```
git diff STRUTS_2_3_16 STRUTS_2_3_16_3
git diff <tagname>
git diff <commitID> <path>
git diff e6e4dcd bin/
git diff --name-only      # show only names of changed files.

git diff --diff-filter=M  # Select only files that are Modified(M) / Added(A),Copied(C),Deleted(D),Renamed(R) etc.

git diff			# diff working_tree <-> staged
git diff --cached	# diff                  staged <-> last_commit(HEAD)
git diff HEAD      	# diff working_tree <------------> last_commit(HEAD)

git diff --color

git diff --shortstat
 75 files changed, 76 insertions(+), 136 deletions(-)

git diff --numstat
1       1       pathnames.h
1       60      sh.c

```

#### stash

- ワークエリアの変更を一時的に退避する機能

```
# 変更をスタックにpush(保存)
git stash         # save work space and index file which is unindexed.
git stash save "massage"
git stash save 
git stash -k      # save work space only without index.
git stash -u      # save with untracked file (not add file)

git stash list    # スタックにある変更を一覧表示する

git stash show            # list modified file
git stash show stash@{#}  
git stash show stash@{#} -p   # list modified file with details

git stash drop            # スタックの一番上にある退避データを削除する
git stash drop stash@{#}  # 指定した退避データを削除する

git stash pop     # スタックから最の変更を一つ取り出して、削除する
git stash pop stash@{#}   # 変の指定

git stash apply   # スタックから最の変更を一つ取り出す

$ git diff stash@{0}
$ git diff stash@{0} hoge.txt
```

# tools

### cvs2git

- CVSからgitへの移行ツール

- 参照URL：　http://qiita.com/uzyura/items/0d444457d361574cd296

- 利用するツール：　cvs2git
  - top: http://cvs2svn.tigris.org/
  - document: http://cvs2svn.tigris.org/cvs2svn.html
  - document: http://www.mcs.anl.gov/~jacob/cvs2svn/cvs2git.html
  - download: http://cvs2svn.tigris.org/servlets/ProjectDocumentList?folderID=2976&expandFolder=2976&folderID=2976

- directory structure

  - /tmp/work
    - cvs2svn-2.4.0/ : build
    
    - cvsrepo/ : original cvs repo
      - CVSROOT/
      - <project_dir>/
      - <project_dir>/

    - git-blob.dat : create by cvs2git command
    - git-dump.dat : create by cvs2git command

    - gitproj/ : git local repository
      - .git/
      - CVSROOT/
      - <project_dir>/
      - <project_dir>/

  - https://github.com/<user_name>/<proj_name>.git : master GIT repo
    
#### ツールの準備

- 上記ダウンロードサイトから、パケージをダウンロードして展開する。

  ```
  $ mkdir -p /tmp/work
  $ gzip -dc cvs2svn-2.4.0.tar.gz |tar xf -
  $ ls cvs2svn-2.4.0/
  ```
- make する場合：
  - http://cvs2svn.tigris.org/servlets/ProjectDocumentList ここから cvs2svn をダウンロードする
  - 解凍して適当に make install

#### CVSデータの準備

- cvs checkout の状態ではだめなため、CVSのレポジトリ以下のファイル（\*.v等）が必要

- CVSレポジトリ内の全部のプロジェクトを移行する場合は、次の手順はスキップする。

- CVSレポジトリ内の特定のプロジェクトだけを移行する場合
  - CVSレポジトリの、CVSROOTと移行したいリポジトリのディレクトリ以下をローカルにコピーしてくる

    ```
    $ mkdir -p /tmp/work/cvsrepo/
    $ scp -r -P 22 <user>@<cvsserver>:/data/cvs/CVSROOT     /tmp/work/cvsrepo/
    $ scp -r -P 22 <user>@<cvsserver>:/data/cvs/ProjectName /tmp/work/cvsrepo/
    ```

#### CVSリポジトリのダンプ

- ダンプ先のディレクトリ以下で以下を実行

  ```
  $ cd /tmp/work
  $ ./cvs2svn-2.4.0/cvs2git --encoding=utf-8 --blobfile=git-blob.dat --dumpfile=git-dump.dat --username=<your_uname> ./cvsrepo/
  $ ls -lF
  total 4948
  drwxrwxr-x 4 vagrant vagrant    4096 Jul 25 07:07 ./
  drwxrwxrwt 5 root    root       4096 Jul 25 07:07 ../
  drwxrwxr-x 9 vagrant vagrant    4096 Sep 22  2012 cvs2svn-2.4.0/
  -rw-rw-r-- 1 vagrant vagrant  514891 Sep 22  2012 cvs2svn-2.4.0.tar.gz
  drwxrwxr-x 4 vagrant vagrant    4096 Jul 25 07:06 cvsrepo/
  -rw-rw-r-- 1 vagrant vagrant 4342570 Jul 25 07:07 git-blob.dat
  -rw-rw-r-- 1 vagrant vagrant  187865 Jul 25 07:07 git-dump.dat
  ```

#### 移行先のGitリポジトリの作成

- CVSリポジトリの移行先となるGitリポジトリを作成します。
- gitユーザーでGitサーバーに接続し、cvs2git.gitベアリポジトリを作成します。
- また、ssh-agentを起動し、SSH接続時のパスフレーズ入力をバイパスします。

  ```
  $ cd /tmp/work
  $ git init gitproj
  ```

#### CVSリポジトリのインポート

- 先程作成したダンプしたデータを、Gitリポジトリにインポートします。

  ```
  $ cd /tmp/work/gitproj
  $ cat ../git-blob.dat ../git-dump.dat | git fast-import
  git-fast-import statistics:
  		  :
  ---------------------------------------------------------------------
  pack_report: getpagesize()            =       4096
  pack_report: core.packedGitWindowSize = 1073741824
  pack_report: core.packedGitLimit      = 8589934592
  pack_report: pack_used_ctr            =         53
  pack_report: pack_mmap_calls          =         13
  pack_report: pack_open_windows        =          1 /          1
  pack_report: pack_mapped              =    3586990 /    3586990
  ---------------------------------------------------------------------
  $ ls
  ./  ../  .git/

  # checkout soruce files
  $ git checkout
  $ ls
  ./  ../  .git/  CVSROOT/  assess-tools-bin/  mktool/  tools/
  
  # remove CVSROOT
  $ git rm -r CVSROOT
  $ git commit -m 'DELETE CVSROOT'
  ```


# specification（仕様）

#### data structure

```
リモートリポジトリ　＜--＞ ローカルリポジトリ　＜--　ステージングエリア　＜-- 　ワークツリー
```

- リポジトリにはリモートとローカルがある。

- ローカルリポジトリへの登録

  - ワークツリーからステージングエリアにファイルを追加した後に、ローカルリポジトリにコミットする必要がある。

- ref: https://marklodato.github.io/visual-git-guide/index-ja.html

- Remote Repository
  - History

  ^ git push
  v git pull / git clone

- Local Repository

  - History

    ^ git commit
    v git reset -- files / -p 

  - Stage ( Index )

    ^ git add files [-p]
    v git checkout -- files / -p

  - Working Tree

- ローカルリポジトリの作成

  - git init

#### definition（用語）

- リポジトリ
  
  - ローカルリポジトリ
  
    - ユーザーの作業領域である「Working Tree」を保持
    - git addやgit commitコマンドの対象となるリポジトリ。
    
  - リモートリポジトリ

    - 外部にあるリポジトリ
      - ローカルリポジトリから見て外部にあれば、リモートに存在しなくてもよい
    - デフォルトでoriginという名前で登録される。
    - 複数人で作業する場合、インターネットに公開する場合などに利用する
    - 原則として、リモートリポジトリはWorking Treeを伴わないbareリポジトリとして作成する

- rigin
  - リモートリポジトリの場所（URL）の別名
  - デフォルトのリモートリポジトリのこと

- master
  - ブランチの名前
  - デフォルトブランチのこと
  - リポジトリ作成時に最初に作成されるブランチ

- HEAD
  - ユーザが作業しているローカルブランチへのポインタ

- Working Tree(working tree)
  - 作業領域
  - リポジトリから取り出したファイルが置かれ、ユーザーが作業する場所。

- remote
  - リモートリポジトリのこと、またはリモートリポジトリに接続するための定義のこと
  - 定義は、ローカルリポジトリの.git/configにremoteセクションとして書かれている
  - definition:
    ```
    [remote "origin"]
        fetch = +refs/heads/*:refs/remotes/origin/*
        url = git@github.com:kaitoy/blog.git
    ```
    - origin
      - リモート名
    - fetch:
      - リモートリポジトリ内のブランチとローカルリポジトリ内の追跡ブランチの対応の定義（refspecと呼ばれる）
      - 上の例:
      	- リモートリポジトリの.git/refs/heads/にある全てのブランチを、ローカルリポジトリの.git/refs/remotes/origin/にある同名のブランチで追跡する、という意味。
    - url:
      - リモートリポジトリのURL
      - リモートリポジトリの場所と、データをやり取りするプロトコルの定義
      - 種類：
        - ファイルパス：非推奨：/path/to/repo.git
	- ファイルURL：file:///path/to/repo.git
	- HTTP(S) : https://github.com/u_name/xxxx.git
	- git : git://github.com/path/to/repo.git
	- ssh + git : ssh://git@github.com/u_name/xxxx.git
	
- ブランチ(branch)
  - Gitには3種類のブランチがある

  - ローカルブランチ(local branch)
    - ローカルリポジトリで管理されるブランチ。
    - git initでリポジトリを作成すると自動的にmasterブランチが作成される。

  - リモートブランチ(remote branch)
    - リモートリポジトリにあるブランチ
    - git cloneでリモートリポジトリをローカルにコピーした場合、リモートブランチのmasterがコピーされる。

  - リモートトラッキングブランチ（remote-tracking branch）（リモート追跡ブランチ）

    - ローカルリポジトリにあって、他の（リモート）リポジトリの状態を追跡するためのブランチ
    - git fetchすると更新されるブランチ
      - git pull = ( git fetch &  git merge )
    - origin/masterは、リモートリポジトリoriginのmasterブランチを追っているリモート追跡ブランチ
    - リモートブランチからローカルブランチにチェックアウトすると自動的に作成される。
    - ローカルリポジトリの.git/refs/remotes/に格納される
    - git branch -r で一覧が見れる
    
    - local branchの内容をリモートに反映するときに、どのブランチに反映するかを指定するためのbranchの仕組み
      - git push [-u | --set-upstream] origin <remote_branchname>
	  ```
	  $ git branch -a -vv .... デフォルトのremote (upstream) branch名も表示する。
	  * fix1                  eaccfe8 [origin/fix1] fix
		master                3d16af3 [origin/master] new
		remotes/origin/HEAD   -> origin/master
		remotes/origin/fix1   eaccfe8 fix
		remotes/origin/master 3d16af3 new
	  ```
	  
    - リモートブランチを参照するために利用する。
    - このブランチは変更できない。
    - 「origin/master」というのは、originという名前のリモートリポジトリにあるmasterブランチをトラッキングしていることを意味する。

  - 上流ブランチ（upstream branch)
    - ローカルリポジトリから見たリモート追跡ブランチのこと。


- チェックアウト(check out)
  - リポジトリに存在するブランチをWorking Treeに展開すること。

- コミット(commit)
  - ファイルの変更をリポジトリに反映させること
  - Gitでは二段階の作業が必要
    - Working Tree --> インデックス
      - 「インデックスと呼ばれる一時領域に次のコミット対象として登録する」
    - インデックス --> リポジトリ
      - 「その登録対象をリポジトリにコミットする」
  - GitではWorking Treeとリポジトリの間に、インデックス領域（ステージング）がある。

## git status

  - working treeの状態を表示する。

  - 状態は以下のgroupに分けて報告される。
  
    - Your branch
      - is up to date with 'origin/master'.
      	- remoteのoriginと同じ状態
	- OK
      - is ahead of 'origin/master' by X commit.
      	- X commitだけ、remoteのoriginより新しい状態
	- NEED : git push
  
    - Changes to be committed:
      - Indexと現在のHEADコミットの間に違いのあるファイル
      - コミット対象のファイル
      - NEED : git commit

    - Changes not staged for commit:
      - Working TreeとIndexの間に違いのあるファイル
      - NEED : git add
      
    - Untracked files:
      - Gitで追跡されていないWorking Tree内のファイル
      - git ignoreでは無視されないファイル
      - NEED : git add

  - Changes XXXXは、各ファイルの状態で以下のように分かれる。
  
    - Changes to be committed:
  
      - new file:
        - git addコマンドで、新規に追跡対象になったファイル

      - modified:
        - コミット対象となる修正されたファイル
        - 追跡対象となっているファイルが修正されたあと、git addコマンドでステージングされた状態

    - Changes not staged for commit:
      - modified:
        - 追跡対象のファイルが作業ディレクトリ内で変更されたけれどもまだステージされていない状態
        - 追跡対象となっているファイルを変更するとこの状態になる。

  - git stauts -s : Short Format
    - format : XY PATH1 -> PATH2
      - PATH1 : HEAD内でのファイルパス
      - PATH2 : IndexとWorking Treeの間で異なる場合に表示される。（renameした場合）
      - XY : status code
        - X : status of Index
	- Y : status of Working Tree
      - status:
      	- ' ' : unmodified
	- M   : modified
	- A   : added
	- D   : deleted
	- R   : renamed
	- C   : copied
	- U   : updated but unmerged
	- ?   : untracked
	- !   : ignored

```
$ git config -l
remote.<repo_name>.url=https://....
remote.<repo_name>.fetch=+refs/heads/*:refs/remotes/<repo_name>/*
$ git remote add fooX https://...
remote.fooX.url=....
remote.fooX.fetch=...
```

#### 仕組み

- 参照URL:
  - https://git-scm.com/book/ja/v2/Git-%E3%81%AE%E3%83%96%E3%83%A9%E3%83%B3%E3%83%81%E6%A9%9F%E8%83%BD-%E3%83%96%E3%83%A9%E3%83%B3%E3%83%81%E3%81%A8%E3%81%AF

- コミット（commit）の仕組み

- 差分としてデータを保持しているのではありません。そうではなく、スナップショットとして保持
  - スナップショットの構造
    - commit object
      - tree object（複数のblob objectのリスト）
        - blob object(fileの中身)
        - blob object(fileの中身)
        - ...
  - コミットを繰り返すと、スナップショットが次々に作成され、それぞれがリスト構造で繋がる。
    - commit objectにparent menberがあり、このポインタで親オブジェクトをたどることができる。

- ブランチ（branch）の仕組み

- ブランチは、コミットのスナップショットを表現commit objectへのポインタとして表現される。
    - masterブランチは、git init コマンドがデフォルトで最初に作成されるブランチであるので、他のブランチとの違いは、無い。
  - 新しいブランチをgit branch testingで作成すると、testingという名前のポインタが追加で作成されるだけ。
  - ユーザが作業している場所は、HEADというポインタで示される。
  - HEADは、ユーザが作業しているローカルブランチへのポインタ

- マージ（merge）の仕組み

- マージ先のブランチが示すコミットが、マージ元のコミットの直接の親であるときにマージ処理するには、 単に、マジ元のブランチのポインタをマージ先と同じにするだけ。これを、fast-forwad マージと呼ぶ。
    ```
    $ git checkout master .... move to master branch
    $ git branch hotfix ...... create hotfix branch
    $ git checkout hotfix .... move to hotfix branch ( = git checkout -b hotfix : with creating branch operation )
    $ vi files
    $ git commit -a -m 'fixed the problem'
    $ git checkout master
    $ git mere hotfix
    # Fast-forwad
    ```
  - 分岐したブランチのマージ
	- iss53の修正内容をmasterにマージする場合の手順概要
	  - iss53の内容を修正
	  - masterブランチに移動（checkout）
	  - masterブランチでiss53をmergeする。

    ```
    $ git checkout iss53
    $ vi file
    $ git commit -a -m 'finished issue 53 [issue 53]'
    $ git checkout master
    $ git merge iss53

    # コンフリクトが発生した場合
    $ git merge iss53
    Auto-merging index.html
    CONFLICT (content): Merge conflict in index.html
    Automatic merge failed; fix conflicts and then commit the result.

    $ git status
    On branch master
    You have unmerged paths.
    (fix conflicts and run "git commit")

    Unmerged paths:
    (use "git add <file>..." to mark resolution)

    both modified:      index.html

    no changes added to commit (use "git add" and/or "git commit -a")

    $ vi index.html
    iss53:index.html

    $ git add index.html

    $ git status
    On branch master
    All conflicts fixed but you are still merging.
    (use "git commit" to conclude merge)

    $ git commit

    ```

#### TIPS

- パスワード(password)を入力せずにPUSHする方法：

  ```
  $ git remote set-url origin git@github.com:hiromzn/study1
  $ git remote -v
  origin  git@github.com:hiromzn/study1 (fetch)
  origin  git@github.com:hiromzn/study1 (push)
  ```

- debug logの出力方法

  ```
  GIT_CURL_VERBOSE=1 git push
  ```

- ERROR: Please, commit your changes or stash them before you can merge.

```
$ git pull                                                                                                     
error: Your local changes to the following files would be overwritten by merge:
Please, commit your changes or stash them before you can merge.
Aborting

$ git stash

$ git pull
$ git stash pop
```

- ERROR: You must use a personal access token or SSH key

```
ERROR:
	$ git push
	Username for 'https://github.hpe.com':
	Password for 'https://hiromichi-mizuno@github.hpe.com':
	remote: Password authentication is not available for Git operations.
	remote: You must use a personal access token or SSH key.
	remote: See https://github.hpe.com/settings/tokens or https://github.hpe.com/settings/ssh
	fatal: unable to access 'https://github.hpe.com/hiromichi-mizuno/sourceanalyze.git/': The requested URL returned error: 403

CHECK:
	$ git remote -v
	origin  https://github.hpe.com/hiromichi-mizuno/study1 (fetch)
	origin  https://github.hpe.com/hiromichi-mizuno/study1 (push)

## RESOLVE ##
	$ git remote set-url origin git@github.hpe.com:hiromichi-mizuno/sourceanalyze.git
	  or
	using token with http protocol ( *ref: https with TOKEN )
	
	$ git remote -v
	origin  git@github.hpe.com:hiromichi-mizuno/study1 (fetch)
	origin  git@github.hpe.com:hiromichi-mizuno/study1 (push)
```

#### vars
- inventory
- group_vars
- roles
  - common

#### sample operation

- リモートリポジトリをダウンロードする。

  ```
  $ git clone <URL>
  $ git clone -b <branch_name> <URL>
  ```

- 最新の情報を取得する。

  ```
  $ git fetch origin master
  ```

#### access by https

```
$ git clone https://github.com/hiromzn/installer
$ git clone -b <branch_name> https://github.com/hiromzn/installer
```

#### access by ssh

```
$ git clone ssh://git@github.com/hiromzn/installer.git
$ git clone git@github.com:hiromzn/installer
```


#### プロキシの設定：

```
$ git config --global http.proxy http://proxy.jpn.hp.com:8080
$ git config --global https.proxy http://proxy.jpn.hp.com:8080
```

#### Gitとは

- 分散型のバージョン管理システム
- ローカルで開発したものをリモートのリポジトリに置くことによってバージョン管理を行う。

- git reset
	- 要らなくなったコミット(commit)を捨てる

  	- git reset --soft : コミットだけを無かったことにする
  	- git reset --mixed : 変更したインデックスの状態を元に戻す
  	- git reset --hard : 最近のコミットを完全に無かったことにする
	  - git reset --hard HEAD^ : 直前のコミットを破棄してファイルも戻す。
	  - git reset --hard HEAD~{n} : n個のコミットを破棄してファイルも戻す。
	  - git reset --hard <hash> : <hash>のコミットを破棄してファイルも戻す。

  - addしたファイルを取り消す
    - git reset HEAD <file_name>

- ログや状態を見る
	    git log
	    git status

- how to commit

      git add -A
      git commit -m "message"
      git push

    	git pull

    	git log
    	git log --decorate  .... with tag info.

- TAG: タグ

  - list

    ```
    git tag -l
    git tag
    git tag -n[<n>]  ..... print tag message with <n> lines
    ```

  - add（追加）
  
    ```
    git tag <tag_name> .................... 軽量版タグを付与する。
    
    git tag -m "tag message" <tag_name> ...... 注釈付きタグを付与する。
    git tag -a <tag_name> -m "tag message" ... 注釈付きタグを付与する。

    git tag -s <tag_name> -m "tag message" ... 署名付きタグを付与する。

    git push origin --tags  ........ ローカルにある全部のタグをリモートに反映させる。
    git push origin <tag_name> ..... ローカルのタグをリモートに反映させる。
    ```

  - delete
  
    ```
    git tag -d <tag_name> ........... ローカルのタグを削除する。
    git push origin :<tag_name> ..... リモートのタグを削除する。
    ```

  ```
  # 軽量版タグ
  git tag <tag_name>

  # 注釈付きタグ
  git tag -a -m "message" <tag_name>

  git tag -n  ...... show tag name and message list
  git tag -l  ...... show tag name list

  # 古いソースにタグをつける方法
  $ git log --oneline
  1d02d95 patch: pdksh-5.2.14-patches2.patch
  d2eeb8c patch: pdksh-5.2.14-patches1.patch
  256ad5e initial version of pdksh-5.2.14
  $ git tag -a initial -m "message of tag" 256ad5e
  $ git tag -a patch-1 -m "message of tag" d2eeb8c
  $ git tag -a patch-2 -m "message of tag" 1d02d95


  git tag -d <tag_name> ........... localのタグを削除する。
  git push origin :<tag_name> ..... remoteのタグを削除する。
  ```

# Github

- https://github.com/

- リポジトリを公開するためのwebサービス

- 使い方は次の通り

	    ユーザー登録をする
	    リポジトリを作成する
	    リポジトリをローカルにコピーする
	    ローカルのリポジトリにファイルをアップする
	    Githubのリポジトリにアップする

	以上の流れでソースコードを公開します。


	リポジトリをローカルにコピー

		リポジトリを作成したいディレクトリ上で次のコマンドを入力します。

			git clone リポジトリのURL

		リポジトリのURLはgit@github.com:nigohiroki/LightsOut.gitのような
		感じのがリポジトリページの上の方にあるのでそれを入力します。

	Github上のリポジトリにアップ

		次のコマンドを使います。

			git push

		以上を行うとGithubでソースコードを公開することができます。

Gistとは
	GitHub のサービス。ソースコード単体を投稿できるサービス

	単一ファイルで管理、公開ができます。
	ブログの記事ごとのソースをわかりやすく公開することができます。

git tutorial
	initial
    ```
		$ mkdir work
		$ cd work
		$ git init

		$ mkdir struts
		$ cd struts
		$ git clone git://git.apache.org/struts.git
		Cloning into 'struts'...
		remote: Counting objects: 92004, done.
		remote: Compressing objects: 100% (31127/31127), done.
		remote: Total 92004 (delta 48435), reused 87934 (delta 45758)
		Receiving objects: 100% (92004/92004), 58.89 MiB | 230 KiB/s, done.
		Resolving deltas: 100% (48435/48435), done.
    ```

	status
    ```
		$ git log |grep STRUTS_2_3_16
		    [maven-release-plugin] prepare release STRUTS_2_3_16_3
		    [maven-release-plugin] prepare release STRUTS_2_3_16_2
		    [maven-release-plugin] prepare release STRUTS_2_3_16
		    [maven-release-plugin] prepare release STRUTS_2_3_16
    ```
https://windows.github.com/
