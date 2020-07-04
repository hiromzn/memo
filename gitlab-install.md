## install log for CentOS 7

- date : 2019/3/2

```
[root@gitlab-01 hmizuno]# yum install -y curl policycoreutils-python openssh-server
読み込んだプラグイン:fastestmirror, langpacks
Determining fastest mirrors
base                                                           | 3.6 kB  00:00:00     
extras                                                         | 3.4 kB  00:00:00     
openlogic                                                      | 2.9 kB  00:00:00     
updates                                                        | 3.4 kB  00:00:00     
(1/5): base/7/x86_64/group_gz                                  | 166 kB  00:00:00     
(2/5): extras/7/x86_64/primary_db                              | 180 kB  00:00:00     
(3/5): openlogic/7/x86_64/primary_db                           |  90 kB  00:00:00     
(4/5): updates/7/x86_64/primary_db                             | 2.4 MB  00:00:00     
(5/5): base/7/x86_64/primary_db                                | 6.0 MB  00:00:00     
パッケージ openssh-server-7.4p1-16.el7.x86_64 はインストール済みか最新バージョンです
依存性の解決をしています
--> トランザクションの確認を実行しています。
---> パッケージ curl.x86_64 0:7.29.0-46.el7 を 更新
---> パッケージ curl.x86_64 0:7.29.0-51.el7 を アップデート
--> 依存性の処理をしています: libcurl = 7.29.0-51.el7 のパッケージ: curl-7.29.0-51.el7.x86_64
---> パッケージ policycoreutils-python.x86_64 0:2.5-29.el7_6.1 を インストール
--> 依存性の処理をしています: policycoreutils = 2.5-29.el7_6.1 のパッケージ: policycoreutils-python-2.5-29.el7_6.1.x86_64
--> 依存性の処理をしています: setools-libs >= 3.3.8-4 のパッケージ: policycoreutils-python-2.5-29.el7_6.1.x86_64
--> 依存性の処理をしています: libsemanage-python >= 2.5-14 のパッケージ: policycoreutils-python-2.5-29.el7_6.1.x86_64
--> 依存性の処理をしています: audit-libs-python >= 2.1.3-4 のパッケージ: policycoreutils-python-2.5-29.el7_6.1.x86_64
--> 依存性の処理をしています: python-IPy のパッケージ: policycoreutils-python-2.5-29.el7_6.1.x86_64
--> 依存性の処理をしています: libqpol.so.1(VERS_1.4)(64bit) のパッケージ: policycoreutils-python-2.5-29.el7_6.1.x86_64
--> 依存性の処理をしています: libqpol.so.1(VERS_1.2)(64bit) のパッケージ: policycoreutils-python-2.5-29.el7_6.1.x86_64
--> 依存性の処理をしています: libcgroup のパッケージ: policycoreutils-python-2.5-29.el7_6.1.x86_64
--> 依存性の処理をしています: libapol.so.4(VERS_4.0)(64bit) のパッケージ: policycoreutils-python-2.5-29.el7_6.1.x86_64
--> 依存性の処理をしています: checkpolicy のパッケージ: policycoreutils-python-2.5-29.el7_6.1.x86_64
--> 依存性の処理をしています: libqpol.so.1()(64bit) のパッケージ: policycoreutils-python-2.5-29.el7_6.1.x86_64
--> 依存性の処理をしています: libapol.so.4()(64bit) のパッケージ: policycoreutils-python-2.5-29.el7_6.1.x86_64
--> トランザクションの確認を実行しています。
---> パッケージ audit-libs-python.x86_64 0:2.8.4-4.el7 を インストール
--> 依存性の処理をしています: audit-libs(x86-64) = 2.8.4-4.el7 のパッケージ: audit-libs-python-2.8.4-4.el7.x86_64
---> パッケージ checkpolicy.x86_64 0:2.5-8.el7 を インストール
---> パッケージ libcgroup.x86_64 0:0.41-20.el7 を インストール
---> パッケージ libcurl.x86_64 0:7.29.0-46.el7 を 更新
---> パッケージ libcurl.x86_64 0:7.29.0-51.el7 を アップデート
--> 依存性の処理をしています: nss-pem(x86-64) >= 1.0.3-5 のパッケージ: libcurl-7.29.0-51.el7.x86_64
---> パッケージ libsemanage-python.x86_64 0:2.5-14.el7 を インストール
--> 依存性の処理をしています: libsemanage = 2.5-14.el7 のパッケージ: libsemanage-python-2.5-14.el7.x86_64
---> パッケージ policycoreutils.x86_64 0:2.5-22.el7 を 更新
---> パッケージ policycoreutils.x86_64 0:2.5-29.el7_6.1 を アップデート
--> 依存性の処理をしています: libsepol >= 2.5-10 のパッケージ: policycoreutils-2.5-29.el7_6.1.x86_64
--> 依存性の処理をしています: libselinux-utils >= 2.5-14 のパッケージ: policycoreutils-2.5-29.el7_6.1.x86_64
---> パッケージ python-IPy.noarch 0:0.75-6.el7 を インストール
---> パッケージ setools-libs.x86_64 0:3.3.8-4.el7 を インストール
--> 依存性の処理をしています: libselinux >= 2.5-14.1 のパッケージ: setools-libs-3.3.8-4.el7.x86_64
--> トランザクションの確認を実行しています。
---> パッケージ audit-libs.x86_64 0:2.8.1-3.el7 を 更新
--> 依存性の処理をしています: audit-libs(x86-64) = 2.8.1-3.el7 のパッケージ: audit-2.8.1-3.el7.x86_64
---> パッケージ audit-libs.x86_64 0:2.8.4-4.el7 を アップデート
---> パッケージ libselinux.x86_64 0:2.5-12.el7 を 更新
--> 依存性の処理をしています: libselinux(x86-64) = 2.5-12.el7 のパッケージ: libselinux-python-2.5-12.el7.x86_64
---> パッケージ libselinux.x86_64 0:2.5-14.1.el7 を アップデート
---> パッケージ libselinux-utils.x86_64 0:2.5-12.el7 を 更新
---> パッケージ libselinux-utils.x86_64 0:2.5-14.1.el7 を アップデート
---> パッケージ libsemanage.x86_64 0:2.5-11.el7 を 更新
---> パッケージ libsemanage.x86_64 0:2.5-14.el7 を アップデート
---> パッケージ libsepol.x86_64 0:2.5-8.1.el7 を 更新
---> パッケージ libsepol.x86_64 0:2.5-10.el7 を アップデート
---> パッケージ nss-pem.x86_64 0:1.0.3-4.el7 を 更新
---> パッケージ nss-pem.x86_64 0:1.0.3-5.el7 を アップデート
--> トランザクションの確認を実行しています。
---> パッケージ audit.x86_64 0:2.8.1-3.el7 を 更新
---> パッケージ audit.x86_64 0:2.8.4-4.el7 を アップデート
---> パッケージ libselinux-python.x86_64 0:2.5-12.el7 を 更新
---> パッケージ libselinux-python.x86_64 0:2.5-14.1.el7 を アップデート
--> 依存性解決を終了しました。

依存性を解決しました

======================================================================================
 Package                      アーキテクチャー
                                           バージョン             リポジトリー   容量
======================================================================================
インストール中:
 policycoreutils-python       x86_64       2.5-29.el7_6.1         updates       456 k
更新します:
 curl                         x86_64       7.29.0-51.el7          base          269 k
依存性関連でのインストールをします:
 audit-libs-python            x86_64       2.8.4-4.el7            base           76 k
 checkpolicy                  x86_64       2.5-8.el7              base          295 k
 libcgroup                    x86_64       0.41-20.el7            base           66 k
 libsemanage-python           x86_64       2.5-14.el7             base          113 k
 python-IPy                   noarch       0.75-6.el7             base           32 k
 setools-libs                 x86_64       3.3.8-4.el7            base          620 k
依存性関連での更新をします:
 audit                        x86_64       2.8.4-4.el7            base          250 k
 audit-libs                   x86_64       2.8.4-4.el7            base          100 k
 libcurl                      x86_64       7.29.0-51.el7          base          221 k
 libselinux                   x86_64       2.5-14.1.el7           base          162 k
 libselinux-python            x86_64       2.5-14.1.el7           base          235 k
 libselinux-utils             x86_64       2.5-14.1.el7           base          151 k
 libsemanage                  x86_64       2.5-14.el7             base          151 k
 libsepol                     x86_64       2.5-10.el7             base          297 k
 nss-pem                      x86_64       1.0.3-5.el7            base           74 k
 policycoreutils              x86_64       2.5-29.el7_6.1         updates       916 k

トランザクションの要約
======================================================================================
インストール  1 パッケージ (+ 6 個の依存関係のパッケージ)
更新          1 パッケージ (+10 個の依存関係のパッケージ)

総ダウンロード容量: 4.4 M
Downloading packages:
Delta RPMs disabled because /usr/bin/applydeltarpm not installed.
(1/18): audit-libs-2.8.4-4.el7.x86_64.rpm                      | 100 kB  00:00:00     
(2/18): audit-2.8.4-4.el7.x86_64.rpm                           | 250 kB  00:00:00     
(3/18): audit-libs-python-2.8.4-4.el7.x86_64.rpm               |  76 kB  00:00:00     
(4/18): curl-7.29.0-51.el7.x86_64.rpm                          | 269 kB  00:00:00     
(5/18): libcgroup-0.41-20.el7.x86_64.rpm                       |  66 kB  00:00:00     
(6/18): libcurl-7.29.0-51.el7.x86_64.rpm                       | 221 kB  00:00:00     
(7/18): libselinux-2.5-14.1.el7.x86_64.rpm                     | 162 kB  00:00:00     
(8/18): checkpolicy-2.5-8.el7.x86_64.rpm                       | 295 kB  00:00:00     
(9/18): libselinux-python-2.5-14.1.el7.x86_64.rpm              | 235 kB  00:00:00     
(10/18): libselinux-utils-2.5-14.1.el7.x86_64.rpm              | 151 kB  00:00:00     
(11/18): libsemanage-2.5-14.el7.x86_64.rpm                     | 151 kB  00:00:00     
(12/18): libsepol-2.5-10.el7.x86_64.rpm                        | 297 kB  00:00:00     
(13/18): nss-pem-1.0.3-5.el7.x86_64.rpm                        |  74 kB  00:00:00     
(14/18): libsemanage-python-2.5-14.el7.x86_64.rpm              | 113 kB  00:00:00     
(15/18): python-IPy-0.75-6.el7.noarch.rpm                      |  32 kB  00:00:00     
(16/18): policycoreutils-2.5-29.el7_6.1.x86_64.rpm             | 916 kB  00:00:00     
(17/18): policycoreutils-python-2.5-29.el7_6.1.x86_64.rpm      | 456 kB  00:00:00     
(18/18): setools-libs-3.3.8-4.el7.x86_64.rpm                   | 620 kB  00:00:00     
--------------------------------------------------------------------------------------
合計                                                     7.7 MB/s | 4.4 MB  00:00     
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  更新します              : libsepol-2.5-10.el7.x86_64                           1/29 
  更新します              : libselinux-2.5-14.1.el7.x86_64                       2/29 
  更新します              : audit-libs-2.8.4-4.el7.x86_64                        3/29 
  更新します              : libsemanage-2.5-14.el7.x86_64                        4/29 
  インストール中          : libsemanage-python-2.5-14.el7.x86_64                 5/29 
  インストール中          : audit-libs-python-2.8.4-4.el7.x86_64                 6/29 
  インストール中          : setools-libs-3.3.8-4.el7.x86_64                      7/29 
  更新します              : libselinux-python-2.5-14.1.el7.x86_64                8/29 
  更新します              : libselinux-utils-2.5-14.1.el7.x86_64                 9/29 
  更新します              : policycoreutils-2.5-29.el7_6.1.x86_64               10/29 
  インストール中          : python-IPy-0.75-6.el7.noarch                        11/29 
  更新します              : nss-pem-1.0.3-5.el7.x86_64                          12/29 
  更新します              : libcurl-7.29.0-51.el7.x86_64                        13/29 
  インストール中          : checkpolicy-2.5-8.el7.x86_64                        14/29 
  インストール中          : libcgroup-0.41-20.el7.x86_64                        15/29 
  インストール中          : policycoreutils-python-2.5-29.el7_6.1.x86_64        16/29 
  更新します              : curl-7.29.0-51.el7.x86_64                           17/29 
  更新します              : audit-2.8.4-4.el7.x86_64                            18/29 
  整理中                  : policycoreutils-2.5-22.el7.x86_64                   19/29 
  整理中                  : libsemanage-2.5-11.el7.x86_64                       20/29 
  整理中                  : libselinux-utils-2.5-12.el7.x86_64                  21/29 
  整理中                  : curl-7.29.0-46.el7.x86_64                           22/29 
  整理中                  : libselinux-python-2.5-12.el7.x86_64                 23/29 
  整理中                  : libselinux-2.5-12.el7.x86_64                        24/29 
  整理中                  : audit-2.8.1-3.el7.x86_64                            25/29 
  整理中                  : audit-libs-2.8.1-3.el7.x86_64                       26/29 
  整理中                  : libsepol-2.5-8.1.el7.x86_64                         27/29 
  整理中                  : libcurl-7.29.0-46.el7.x86_64                        28/29 
  整理中                  : nss-pem-1.0.3-4.el7.x86_64                          29/29 
  検証中                  : libcgroup-0.41-20.el7.x86_64                         1/29 
  検証中                  : libcurl-7.29.0-51.el7.x86_64                         2/29 
  検証中                  : policycoreutils-2.5-29.el7_6.1.x86_64                3/29 
  検証中                  : checkpolicy-2.5-8.el7.x86_64                         4/29 
  検証中                  : audit-libs-2.8.4-4.el7.x86_64                        5/29 
  検証中                  : audit-2.8.4-4.el7.x86_64                             6/29 
  検証中                  : nss-pem-1.0.3-5.el7.x86_64                           7/29 
  検証中                  : python-IPy-0.75-6.el7.noarch                         8/29 
  検証中                  : setools-libs-3.3.8-4.el7.x86_64                      9/29 
  検証中                  : policycoreutils-python-2.5-29.el7_6.1.x86_64        10/29 
  検証中                  : libsemanage-python-2.5-14.el7.x86_64                11/29 
  検証中                  : libsemanage-2.5-14.el7.x86_64                       12/29 
  検証中                  : libsepol-2.5-10.el7.x86_64                          13/29 
  検証中                  : libselinux-python-2.5-14.1.el7.x86_64               14/29 
  検証中                  : audit-libs-python-2.8.4-4.el7.x86_64                15/29 
  検証中                  : libselinux-utils-2.5-14.1.el7.x86_64                16/29 
  検証中                  : curl-7.29.0-51.el7.x86_64                           17/29 
  検証中                  : libselinux-2.5-14.1.el7.x86_64                      18/29 
  検証中                  : libsemanage-2.5-11.el7.x86_64                       19/29 
  検証中                  : libselinux-python-2.5-12.el7.x86_64                 20/29 
  検証中                  : nss-pem-1.0.3-4.el7.x86_64                          21/29 
  検証中                  : curl-7.29.0-46.el7.x86_64                           22/29 
  検証中                  : audit-libs-2.8.1-3.el7.x86_64                       23/29 
  検証中                  : policycoreutils-2.5-22.el7.x86_64                   24/29 
  検証中                  : libcurl-7.29.0-46.el7.x86_64                        25/29 
  検証中                  : audit-2.8.1-3.el7.x86_64                            26/29 
  検証中                  : libsepol-2.5-8.1.el7.x86_64                         27/29 
  検証中                  : libselinux-2.5-12.el7.x86_64                        28/29 
  検証中                  : libselinux-utils-2.5-12.el7.x86_64                  29/29 

インストール:
  policycoreutils-python.x86_64 0:2.5-29.el7_6.1                                      

依存性関連をインストールしました:
  audit-libs-python.x86_64 0:2.8.4-4.el7    checkpolicy.x86_64 0:2.5-8.el7           
  libcgroup.x86_64 0:0.41-20.el7            libsemanage-python.x86_64 0:2.5-14.el7   
  python-IPy.noarch 0:0.75-6.el7            setools-libs.x86_64 0:3.3.8-4.el7        

更新:
  curl.x86_64 0:7.29.0-51.el7                                                         

依存性を更新しました:
  audit.x86_64 0:2.8.4-4.el7                audit-libs.x86_64 0:2.8.4-4.el7          
  libcurl.x86_64 0:7.29.0-51.el7            libselinux.x86_64 0:2.5-14.1.el7         
  libselinux-python.x86_64 0:2.5-14.1.el7   libselinux-utils.x86_64 0:2.5-14.1.el7   
  libsemanage.x86_64 0:2.5-14.el7           libsepol.x86_64 0:2.5-10.el7             
  nss-pem.x86_64 0:1.0.3-5.el7              policycoreutils.x86_64 0:2.5-29.el7_6.1  

完了しました!
[root@gitlab-01 hmizuno]# systemctl enable sshd
[root@gitlab-01 hmizuno]# systemctl start sshd
[root@gitlab-01 hmizuno]# firewall-cmd --pernot running
manot running
^C
[root@gitlab-01 hmizuno]# firewall-cmd --permanent --add-senot running
^C
[root@gitlab-01 hmizuno]# firewall-cmd --permanent --add-service=http
FirewallD is not running
[root@gitlab-01 hmizuno]# systemctl reload firewalld
Job for firewalld.service invalid.
[root@gitlab-01 hmizuno]# yum install postfix
読み込んだプラグイン:fastestmirror, langpacks
Loading mirror speeds from cached hostfile
依存性の解決をしています
--> トランザクションの確認を実行しています。
---> パッケージ postfix.x86_64 2:2.10.1-6.el7 を 更新
---> パッケージ postfix.x86_64 2:2.10.1-7.el7 を アップデート
--> 依存性解決を終了しました。

依存性を解決しました

======================================================================================
 Package            アーキテクチャー  バージョン                リポジトリー     容量
======================================================================================
更新します:
 postfix            x86_64            2:2.10.1-7.el7            base            2.4 M

トランザクションの要約
======================================================================================
更新  1 パッケージ

総ダウンロード容量: 2.4 M
Is this ok [y/d/N]: y
Downloading packages:
Delta RPMs disabled because /usr/bin/applydeltarpm not installed.
postfix-2.10.1-7.el7.x86_64.rpm                                | 2.4 MB  00:00:07     
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  更新します              : 2:postfix-2.10.1-7.el7.x86_64                         1/2 
  整理中                  : 2:postfix-2.10.1-6.el7.x86_64                         2/2 
  検証中                  : 2:postfix-2.10.1-7.el7.x86_64                         1/2 
  検証中                  : 2:postfix-2.10.1-6.el7.x86_64                         2/2 

更新:
  postfix.x86_64 2:2.10.1-7.el7                                                       

完了しました!
[root@gitlab-01 hmizuno]# systemctl enable postfix
[root@gitlab-01 hmizuno]# systemctl start postfix
[root@gitlab-01 hmizuno]# curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.rpm.sh | sudo bash
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  6564    0  6564    0     0  12083      0 --:--:-- --:--:-- --:--:-- 12088
Detected operating system as centos/7.
Checking for curl...
Detected curl...
Downloading repository file: https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/config_file.repo?os=centos&dist=7&source=script
done.
Installing pygpgme to verify GPG signatures...
読み込んだプラグイン:fastestmirror, langpacks
Loading mirror speeds from cached hostfile
gitlab_gitlab-ee-source/signature                              |  836 B  00:00:00     
https://packages.gitlab.com/gitlab/gitlab-ee/gpgkey から鍵を取得中です。
Importing GPG key 0xE15E78F4:
 Userid     : "GitLab B.V. (package repository signing key) <packages@gitlab.com>"
 Fingerprint: 1a4c 919d b987 d435 9396 38b9 1421 9a96 e15e 78f4
 From       : https://packages.gitlab.com/gitlab/gitlab-ee/gpgkey
https://packages.gitlab.com/gitlab/gitlab-ee/gpgkey/gitlab-gitlab-ee-3D645A26AB9FBD22.pub.gpg から鍵を取得中です。
gitlab_gitlab-ee-source/signature                              |  951 B  00:00:00 !!! 
gitlab_gitlab-ee-source/primary                                |  175 B  00:00:00     
パッケージ pygpgme-0.3-9.el7.x86_64 はインストール済みか最新バージョンです
何もしません
Installing yum-utils...
読み込んだプラグイン:fastestmirror, langpacks
Loading mirror speeds from cached hostfile
依存性の解決をしています
--> トランザクションの確認を実行しています。
---> パッケージ yum-utils.noarch 0:1.1.31-46.el7_5 を 更新
---> パッケージ yum-utils.noarch 0:1.1.31-50.el7 を アップデート
--> 依存性解決を終了しました。

依存性を解決しました

======================================================================================
 Package             アーキテクチャー バージョン                 リポジトリー    容量
======================================================================================
更新します:
 yum-utils           noarch           1.1.31-50.el7              base           121 k

トランザクションの要約
======================================================================================
更新  1 パッケージ

総ダウンロード容量: 121 k
Downloading packages:
Delta RPMs disabled because /usr/bin/applydeltarpm not installed.
yum-utils-1.1.31-50.el7.noarch.rpm                             | 121 kB  00:00:00     
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  更新します              : yum-utils-1.1.31-50.el7.noarch                        1/2 
  整理中                  : yum-utils-1.1.31-46.el7_5.noarch                      2/2 
  検証中                  : yum-utils-1.1.31-50.el7.noarch                        1/2 
  検証中                  : yum-utils-1.1.31-46.el7_5.noarch                      2/2 

更新:
  yum-utils.noarch 0:1.1.31-50.el7                                                    

完了しました!
Generating yum cache for gitlab_gitlab-ee...
Importing GPG key 0xE15E78F4:
 Userid     : "GitLab B.V. (package repository signing key) <packages@gitlab.com>"
 Fingerprint: 1a4c 919d b987 d435 9396 38b9 1421 9a96 e15e 78f4
 From       : https://packages.gitlab.com/gitlab/gitlab-ee/gpgkey

The repository is setup! You can now install packages.
[root@gitlab-01 hmizuno]# EXTERNAL_URL="http://_##_YOUR_HOST.DOMAIN.COM_##_" yum install -y gitlab-ee
読み込んだプラグイン:fastestmirror, langpacks
Loading mirror speeds from cached hostfile
依存性の解決をしています
--> トランザクションの確認を実行しています。
---> パッケージ gitlab-ee.x86_64 0:11.8.0-ee.0.el7 を インストール
--> 依存性解決を終了しました。

依存性を解決しました

======================================================================================
 Package          アーキテクチャー
                                バージョン              リポジトリー             容量
======================================================================================
インストール中:
 gitlab-ee        x86_64        11.8.0-ee.0.el7         gitlab_gitlab-ee        556 M

トランザクションの要約
======================================================================================
インストール  1 パッケージ

総ダウンロード容量: 556 M
インストール容量: 1.5 G
Downloading packages:
警告: /var/cache/yum/x86_64/7/gitlab_gitlab-ee/packages/gitlab-ee-11.8.0-ee.0.el7.x86_64.rpm: ヘッダー V4 RSA/SHA1 Signature、鍵 ID f27eab47: NOKEY
gitlab-ee-11.8.0-ee.0.el7.x86_64.rpm の公開鍵がインストールされていません
gitlab-ee-11.8.0-ee.0.el7.x86_64.rpm                           | 556 MB  00:00:08     
https://packages.gitlab.com/gitlab/gitlab-ee/gpgkey から鍵を取得中です。
Importing GPG key 0xE15E78F4:
 Userid     : "GitLab B.V. (package repository signing key) <packages@gitlab.com>"
 Fingerprint: 1a4c 919d b987 d435 9396 38b9 1421 9a96 e15e 78f4
 From       : https://packages.gitlab.com/gitlab/gitlab-ee/gpgkey
https://packages.gitlab.com/gitlab/gitlab-ee/gpgkey/gitlab-gitlab-ee-3D645A26AB9FBD22.pub.gpg から鍵を取得中です。
Importing GPG key 0xF27EAB47:
 Userid     : "GitLab, Inc. <support@gitlab.com>"
 Fingerprint: dbef 8977 4ddb 9eb3 7d9f c3a0 3cfc f9ba f27e ab47
 From       : https://packages.gitlab.com/gitlab/gitlab-ee/gpgkey/gitlab-gitlab-ee-3D645A26AB9FBD22.pub.gpg
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  インストール中          : gitlab-ee-11.8.0-ee.0.el7.x86_64                      1/1 
It looks like GitLab has not been configured yet; skipping the upgrade script.

       *.                  *.
      ***                 ***
     *****               *****
    .******             *******
    ********            ********
   ,,,,,,,,,***********,,,,,,,,,
  ,,,,,,,,,,,*********,,,,,,,,,,,
  .,,,,,,,,,,,*******,,,,,,,,,,,,
      ,,,,,,,,,*****,,,,,,,,,.
         ,,,,,,,****,,,,,,
            .,,,***,,,,
                ,*,.
  


     _______ __  __          __
    / ____(_) /_/ /   ____ _/ /_
   / / __/ / __/ /   / __ `/ __ \
  / /_/ / / /_/ /___/ /_/ / /_/ /
  \____/_/\__/_____/\__,_/_.___/
  

Thank you for installing GitLab!
GitLab was unable to detect a valid hostname for your instance.
Please configure a URL for your GitLab instance by setting `external_url`
configuration in /etc/gitlab/gitlab.rb file.
Then, you can start your GitLab instance by running the following command:
  sudo gitlab-ctl reconfigure

For a comprehensive list of configuration options please see the Omnibus GitLab readme
https://gitlab.com/gitlab-org/omnibus-gitlab/blob/master/README.md

  検証中                  : gitlab-ee-11.8.0-ee.0.el7.x86_64                      1/1 

インストール:
  gitlab-ee.x86_64 0:11.8.0-ee.0.el7                                                  

完了しました!

## ACCESS: http://_##_YOUR_HOST.DOMAIN.COM_##_

```

- process list

```
[root@gitlab-01 hmizuno]# ps -ef
UID         PID   PPID  C STIME TTY          TIME CMD
root          1      0  0 01:39 ?        00:00:07 /usr/lib/systemd/systemd --system --
root          2      0  0 01:39 ?        00:00:00 [kthreadd]
root          3      2  0 01:39 ?        00:00:00 [ksoftirqd/0]
root          5      2  0 01:39 ?        00:00:00 [kworker/0:0H]
root          6      2  0 01:39 ?        00:00:00 [kworker/u256:0]
root          7      2  0 01:39 ?        00:00:00 [migration/0]
root          8      2  0 01:39 ?        00:00:00 [rcu_bh]
root          9      2  0 01:39 ?        00:00:02 [rcu_sched]
root         10      2  0 01:39 ?        00:00:00 [lru-add-drain]
root         11      2  0 01:39 ?        00:00:00 [watchdog/0]
root         12      2  0 01:39 ?        00:00:00 [watchdog/1]
root         13      2  0 01:39 ?        00:00:00 [migration/1]
root         14      2  0 01:39 ?        00:00:00 [ksoftirqd/1]
root         16      2  0 01:39 ?        00:00:00 [kworker/1:0H]
root         18      2  0 01:39 ?        00:00:00 [kdevtmpfs]
root         19      2  0 01:39 ?        00:00:00 [netns]
root         20      2  0 01:39 ?        00:00:00 [khungtaskd]
root         21      2  0 01:39 ?        00:00:00 [writeback]
root         22      2  0 01:39 ?        00:00:00 [kintegrityd]
root         23      2  0 01:39 ?        00:00:00 [bioset]
root         24      2  0 01:39 ?        00:00:00 [bioset]
root         25      2  0 01:39 ?        00:00:00 [bioset]
root         26      2  0 01:39 ?        00:00:00 [kblockd]
root         27      2  0 01:39 ?        00:00:00 [md]
root         28      2  0 01:39 ?        00:00:00 [edac-poller]
root         34      2  0 01:39 ?        00:00:00 [kswapd0]
root         35      2  0 01:39 ?        00:00:00 [ksmd]
root         36      2  0 01:39 ?        00:00:00 [khugepaged]
root         37      2  0 01:39 ?        00:00:00 [crypto]
root         45      2  0 01:39 ?        00:00:00 [kthrotld]
root         47      2  0 01:39 ?        00:00:00 [kmpath_rdacd]
root         48      2  0 01:39 ?        00:00:00 [kaluad]
root         50      2  0 01:39 ?        00:00:00 [kpsmoused]
root         52      2  0 01:39 ?        00:00:00 [ipv6_addrconf]
root         65      2  0 01:39 ?        00:00:00 [deferwq]
root        155      2  0 01:39 ?        00:00:00 [kauditd]
root        223      2  0 01:39 ?        00:00:00 [hv_vmbus_con]
root        236      2  0 01:39 ?        00:00:00 [ata_sff]
root        241      2  0 01:39 ?        00:00:00 [scsi_eh_0]
root        242      2  0 01:39 ?        00:00:00 [scsi_tmf_0]
root        243      2  0 01:39 ?        00:00:00 [scsi_eh_1]
root        244      2  0 01:39 ?        00:00:00 [scsi_tmf_1]
root        254      2  0 01:39 ?        00:00:00 [scsi_eh_2]
root        255      2  0 01:39 ?        00:00:00 [scsi_tmf_2]
root        256      2  0 01:39 ?        00:00:00 [storvsc_error_w]
root        258      2  0 01:39 ?        00:00:00 [scsi_eh_3]
root        260      2  0 01:39 ?        00:00:00 [scsi_tmf_3]
root        261      2  0 01:39 ?        00:00:00 [storvsc_error_w]
root        263      2  0 01:39 ?        00:00:00 [scsi_eh_4]
root        264      2  0 01:39 ?        00:00:00 [scsi_tmf_4]
root        265      2  0 01:39 ?        00:00:00 [storvsc_error_w]
root        266      2  0 01:39 ?        00:00:00 [scsi_eh_5]
root        268      2  0 01:39 ?        00:00:00 [scsi_tmf_5]
root        269      2  0 01:39 ?        00:00:00 [storvsc_error_w]
root        288      2  0 01:39 ?        00:00:00 [bioset]
root        289      2  0 01:39 ?        00:00:00 [xfsalloc]
root        290      2  0 01:39 ?        00:00:00 [xfs_mru_cache]
root        291      2  0 01:39 ?        00:00:00 [xfs-buf/sda2]
root        292      2  0 01:39 ?        00:00:00 [xfs-data/sda2]
root        293      2  0 01:39 ?        00:00:00 [xfs-conv/sda2]
root        294      2  0 01:39 ?        00:00:00 [xfs-cil/sda2]
root        295      2  0 01:39 ?        00:00:00 [xfs-reclaim/sda]
root        296      2  0 01:39 ?        00:00:00 [xfs-log/sda2]
root        297      2  0 01:39 ?        00:00:00 [xfs-eofblocks/s]
root        298      2  0 01:39 ?        00:00:01 [xfsaild/sda2]
root        299      2  0 01:39 ?        00:00:00 [kworker/0:1H]
root        355      2  0 01:39 ?        00:00:00 [kworker/1:1H]
root        369      1  0 01:39 ?        00:00:02 /usr/lib/systemd/systemd-journald
root        393      1  0 01:39 ?        00:00:00 /usr/sbin/lvmetad -f
root        402      1  0 01:39 ?        00:00:00 /usr/lib/systemd/systemd-udevd
root        425      2  0 01:39 ?        00:00:00 [hv_balloon]
root        443      2  0 01:40 ?        00:00:00 [xfs-buf/sda1]
root        444      2  0 01:40 ?        00:00:00 [xfs-data/sda1]
root        445      2  0 01:40 ?        00:00:00 [xfs-conv/sda1]
root        446      2  0 01:40 ?        00:00:00 [xfs-cil/sda1]
root        447      2  0 01:40 ?        00:00:00 [xfs-reclaim/sda]
root        448      2  0 01:40 ?        00:00:00 [xfs-log/sda1]
root        449      2  0 01:40 ?        00:00:00 [xfs-eofblocks/s]
root        450      2  0 01:40 ?        00:00:00 [xfsaild/sda1]
root        489      2  0 01:40 ?        00:00:00 [kvm-irqfd-clean]
root        555      1  0 01:40 ?        00:00:00 /usr/lib/systemd/systemd-logind
root        556      1  0 01:40 ?        00:00:00 /usr/sbin/hypervvssd -n
dbus        557      1  0 01:40 ?        00:00:00 /usr/bin/dbus-daemon --system --addr
rpc         558      1  0 01:40 ?        00:00:00 /sbin/rpcbind -w
root        561      1  0 01:40 ?        00:00:00 /usr/sbin/irqbalance --foreground
root        563      1  0 01:40 ?        00:00:00 /usr/sbin/smartd -n -q never
polkitd     566      1  0 01:40 ?        00:00:00 /usr/lib/polkit-1/polkitd --no-debug
root        567      1  0 01:40 ?        00:00:00 /sbin/rngd -f
libstor+    572      1  0 01:40 ?        00:00:00 /usr/bin/lsmd -d
root        589      1  0 01:40 ?        00:00:00 /usr/sbin/atd -f
root        598      1  0 01:40 tty1     00:00:00 /sbin/agetty --noclear tty1 linux
root        599      1  0 01:40 ttyS0    00:00:00 /sbin/agetty --keep-baud 115200 3840
chrony      602      1  0 01:40 ?        00:00:00 /usr/sbin/chronyd
root        945      1  0 01:40 ?        00:00:00 /usr/sbin/hypervkvpd -n
root        946      1  0 01:40 ?        00:00:00 /usr/bin/python -Es /usr/sbin/tuned 
root        947      1  0 01:40 ?        00:00:01 /usr/bin/python -u /usr/sbin/waagent
root        950      1  0 01:40 ?        00:00:00 /usr/sbin/rsyslogd -n
root       1082      2  0 01:40 ?        00:00:00 [jbd2/sdb1-8]
root       1083      2  0 01:40 ?        00:00:00 [ext4-rsv-conver]
root       1139      1  0 01:40 ?        00:00:00 /usr/sbin/NetworkManager --no-daemon
root       1326      1  0 01:40 ?        00:00:00 /sbin/dhclient -q -lf /var/lib/dhcli
root       1399      1  0 01:40 ?        00:00:00 /usr/sbin/sshd -D
root       1411    947  0 01:40 ?        00:00:19 python -u bin/WALinuxAgent-2.2.37-py
root       2032      1  0 02:01 ?        00:00:00 /usr/sbin/anacron -s
root      29696      2  0 02:16 ?        00:00:01 [kworker/1:1]
root      29740   1399  0 02:17 ?        00:00:00 sshd: hmizuno [priv]
hmizuno   29743  29740  0 02:17 ?        00:00:00 sshd: hmizuno@pts/0
hmizuno   29746  29743  0 02:17 pts/0    00:00:00 -bash
root      29785  29746  0 02:17 pts/0    00:00:00 sudo bash
root      29793  29785  0 02:17 pts/0    00:00:00 bash
root      29876      1  0 02:18 ?        00:00:00 /usr/sbin/crond -n
root      29918      1  0 02:18 ?        00:00:00 /sbin/auditd
root      30651      1  0 02:20 ?        00:00:00 /usr/libexec/postfix/master -w
postfix   30652  30651  0 02:20 ?        00:00:00 pickup -l -t unix -u
postfix   30653  30651  0 02:20 ?        00:00:00 qmgr -l -t unix -u
root      30672      2  0 02:20 ?        00:00:03 [kworker/u256:1]
root      31088      2  0 02:29 ?        00:00:00 [kworker/1:3]
root      31379      2  0 02:36 ?        00:00:00 [kworker/0:7]
root      31380      2  0 02:36 ?        00:00:00 [kworker/0:8]
root      32528      1  0 02:37 ?        00:00:00 runsvdir -P /opt/gitlab/service log:
root      32586  32528  0 02:37 ?        00:00:00 runsv redis
gitlab-+  32588  32586  0 02:37 ?        00:00:00 /opt/gitlab/embedded/bin/redis-serve
root      32643  32586  0 02:37 ?        00:00:00 svlogd -tt /var/log/gitlab/redis
root      32741  32528  0 02:38 ?        00:00:00 runsv postgresql
gitlab-+  32743  32741  0 02:38 ?        00:00:00 /opt/gitlab/embedded/bin/postgres -D
gitlab-+  32745  32743  0 02:38 ?        00:00:00 postgres: checkpointer process   
gitlab-+  32746  32743  0 02:38 ?        00:00:00 postgres: writer process   
gitlab-+  32747  32743  0 02:38 ?        00:00:00 postgres: wal writer process   
gitlab-+  32748  32743  0 02:38 ?        00:00:00 postgres: autovacuum launcher proces
gitlab-+  32749  32743  0 02:38 ?        00:00:00 postgres: stats collector process   
root      32848  32741  0 02:38 ?        00:00:00 svlogd -tt /var/log/gitlab/postgresq
root      33087  32528  0 02:39 ?        00:00:00 runsv unicorn
git       33089  33087  0 02:39 ?        00:00:00 /bin/bash /opt/gitlab/embedded/bin/g
git       33107      1 22 02:39 ?        00:00:42 unicorn master -D -E production -c /
root      33132  33087  0 02:39 ?        00:00:00 svlogd -tt /var/log/gitlab/unicorn
root      33176  32528  0 02:39 ?        00:00:00 runsv sidekiq
git       33178  33176 24 02:39 ?        00:00:44 sidekiq 5.2.5 gitlab-rails [0 of 25 
root      33201  33176  0 02:39 ?        00:00:00 svlogd -tt /var/log/gitlab/sidekiq
root      33256  32528  0 02:39 ?        00:00:00 runsv gitlab-workhorse
root      33287  33256  0 02:39 ?        00:00:00 svlogd -tt /var/log/gitlab/gitlab-wo
root      33352  32528  0 02:39 ?        00:00:00 runsv nginx
root      33354  33352  0 02:39 ?        00:00:00 nginx: master process /opt/gitlab/em
gitlab-+  33355  33354  0 02:39 ?        00:00:00 nginx: worker process
gitlab-+  33356  33354  0 02:39 ?        00:00:00 nginx: worker process
gitlab-+  33357  33354  0 02:39 ?        00:00:00 nginx: cache manager process
root      33369  33352  0 02:39 ?        00:00:00 svlogd -tt /var/log/gitlab/nginx
root      33414  32528  0 02:39 ?        00:00:00 runsv logrotate
root      33416  33414  0 02:39 ?        00:00:00 /bin/sh /opt/gitlab/embedded/bin/git
root      33420  33416  0 02:39 ?        00:00:00 sleep 600
root      33432  33414  0 02:39 ?        00:00:00 svlogd -tt /var/log/gitlab/logrotate
root      33502  32528  0 02:39 ?        00:00:00 runsv gitaly
root      33537  33502  0 02:39 ?        00:00:00 svlogd -tt /var/log/gitlab/gitaly
root      33696  32528  0 02:39 ?        00:00:00 runsv node-exporter
root      33714  33696  0 02:39 ?        00:00:00 svlogd -tt /var/log/gitlab/node-expo
git       33776  33107  0 02:39 ?        00:00:01 unicorn worker[0] -D -E production -
git       33779  33107  0 02:39 ?        00:00:01 unicorn worker[1] -D -E production -
git       33782  33107  1 02:39 ?        00:00:01 unicorn worker[2] -D -E production -
root      33802  32528  0 02:39 ?        00:00:00 runsv gitlab-monitor
root      33894  33802  0 02:39 ?        00:00:00 svlogd -tt /var/log/gitlab/gitlab-mo
root      33914  32528  0 02:40 ?        00:00:00 runsv redis-exporter
root      33974  33914  0 02:40 ?        00:00:00 svlogd -tt /var/log/gitlab/redis-exp
gitlab-+  34018  32743  0 02:40 ?        00:00:00 postgres: gitlab gitlabhq_production
root      34024  32528  0 02:40 ?        00:00:00 runsv prometheus
root      34084  34024  0 02:40 ?        00:00:00 svlogd -tt /var/log/gitlab/prometheu
root      34112  32528  0 02:40 ?        00:00:00 runsv alertmanager
root      34140  34112  0 02:40 ?        00:00:00 svlogd -tt /var/log/gitlab/alertmana
gitlab-+  34186  32743  0 02:40 ?        00:00:00 postgres: gitlab gitlabhq_production
root      34198  32528  0 02:40 ?        00:00:00 runsv postgres-exporter
root      34250  34198  0 02:40 ?        00:00:00 svlogd -tt /var/log/gitlab/postgres-
git       34294  33256  0 02:40 ?        00:00:00 /opt/gitlab/embedded/bin/gitlab-work
git       34307  33502  0 02:40 ?        00:00:00 /opt/gitlab/embedded/bin/gitaly /var
git       34322  34307  2 02:40 ?        00:00:02 ruby /opt/gitlab/embedded/service/gi
gitlab-+  34328  33696  0 02:40 ?        00:00:00 /opt/gitlab/embedded/bin/node_export
git       34329  34307  2 02:40 ?        00:00:02 ruby /opt/gitlab/embedded/service/gi
git       34347  33802  1 02:40 ?        00:00:01 puma 3.12.0 (tcp://localhost:9168) [
gitlab-+  34355  33914  0 02:40 ?        00:00:00 /opt/gitlab/embedded/bin/redis_expor
gitlab-+  34371  34024  0 02:40 ?        00:00:00 /opt/gitlab/embedded/bin/prometheus 
gitlab-+  34478  34112  0 02:40 ?        00:00:00 /opt/gitlab/embedded/bin/alertmanage
gitlab-+  34495  34198  0 02:40 ?        00:00:00 /opt/gitlab/embedded/bin/postgres_ex
gitlab-+  34500  32743  0 02:40 ?        00:00:00 postgres: gitlab-psql postgres [loca
gitlab-+  34526  32743  0 02:40 ?        00:00:00 postgres: gitlab gitlabhq_production
gitlab-+  34552  32743  0 02:40 ?        00:00:00 postgres: gitlab gitlabhq_production
gitlab-+  34553  32743  0 02:40 ?        00:00:00 postgres: gitlab gitlabhq_production
gitlab-+  34554  32743  0 02:40 ?        00:00:00 postgres: gitlab gitlabhq_production
gitlab-+  34563  32743  0 02:41 ?        00:00:00 postgres: gitlab gitlabhq_production
gitlab-+  34594  32743  0 02:41 ?        00:00:00 postgres: gitlab gitlabhq_production
root      34607      2  0 02:41 ?        00:00:00 [kworker/0:0]
postfix   34641  30651  0 02:41 ?        00:00:00 cleanup -z -t unix -u
postfix   34646  30651  0 02:41 ?        00:00:00 trivial-rewrite -n rewrite -t unix -
postfix   34647  30651  0 02:41 ?        00:00:00 smtp -t unix -u
root      34669      2  0 02:41 ?        00:00:00 [kworker/1:0]
postfix   34714  30651  0 02:42 ?        00:00:00 bounce -z -n defer -t unix -u
git       34741  33089  0 02:42 ?        00:00:00 sleep 1
root      34742  29793  0 02:42 pts/0    00:00:00 ps -ef



[root@gitlab-01 hmizuno]# ps -ef |grep gitla
root       1326      1  0 01:40 ?        00:00:00 /sbin/dhclient -q -lf /var/lib/dhclient/dhclient--eth0.lease -pf /var/run/dhclient-eth0.pid -H gitlab-01 eth0
root      32528      1  0 02:37 ?        00:00:00 runsvdir -P /opt/gitlab/service log: ...........................................................................................................................................................................................................................................................................................................................................................................................................
gitlab-+  32588  32586  0 02:37 ?        00:00:01 /opt/gitlab/embedded/bin/redis-server 127.0.0.1:0
root      32643  32586  0 02:37 ?        00:00:00 svlogd -tt /var/log/gitlab/redis
gitlab-+  32743  32741  0 02:38 ?        00:00:00 /opt/gitlab/embedded/bin/postgres -D /var/opt/gitlab/postgresql/data
gitlab-+  32745  32743  0 02:38 ?        00:00:00 postgres: checkpointer process   
gitlab-+  32746  32743  0 02:38 ?        00:00:00 postgres: writer process   
gitlab-+  32747  32743  0 02:38 ?        00:00:00 postgres: wal writer process   
gitlab-+  32748  32743  0 02:38 ?        00:00:00 postgres: autovacuum launcher process   
gitlab-+  32749  32743  0 02:38 ?        00:00:00 postgres: stats collector process   
root      32848  32741  0 02:38 ?        00:00:00 svlogd -tt /var/log/gitlab/postgresql
git       33089  33087  0 02:39 ?        00:00:00 /bin/bash /opt/gitlab/embedded/bin/gitlab-unicorn-wrapper
git       33107      1 18 02:39 ?        00:00:42 unicorn master -D -E production -c /var/opt/gitlab/gitlab-rails/etc/unicorn.rb /opt/gitlab/embedded/service/gitlab-rails/config.ru
root      33132  33087  0 02:39 ?        00:00:00 svlogd -tt /var/log/gitlab/unicorn
git       33178  33176 20 02:39 ?        00:00:44 sidekiq 5.2.5 gitlab-rails [0 of 25 busy]
root      33201  33176  0 02:39 ?        00:00:00 svlogd -tt /var/log/gitlab/sidekiq
root      33256  32528  0 02:39 ?        00:00:00 runsv gitlab-workhorse
root      33287  33256  0 02:39 ?        00:00:00 svlogd -tt /var/log/gitlab/gitlab-workhorse
root      33354  33352  0 02:39 ?        00:00:00 nginx: master process /opt/gitlab/embedded/sbin/nginx -p /var/opt/gitlab/nginx
gitlab-+  33355  33354  0 02:39 ?        00:00:00 nginx: worker process
gitlab-+  33356  33354  0 02:39 ?        00:00:00 nginx: worker process
gitlab-+  33357  33354  0 02:39 ?        00:00:00 nginx: cache manager process
root      33369  33352  0 02:39 ?        00:00:00 svlogd -tt /var/log/gitlab/nginx
root      33416  33414  0 02:39 ?        00:00:00 /bin/sh /opt/gitlab/embedded/bin/gitlab-logrotate-wrapper
root      33432  33414  0 02:39 ?        00:00:00 svlogd -tt /var/log/gitlab/logrotate
root      33537  33502  0 02:39 ?        00:00:00 svlogd -tt /var/log/gitlab/gitaly
root      33714  33696  0 02:39 ?        00:00:00 svlogd -tt /var/log/gitlab/node-exporter
git       33776  33107  0 02:39 ?        00:00:01 unicorn worker[0] -D -E production -c /var/opt/gitlab/gitlab-rails/etc/unicorn.rb /opt/gitlab/embedded/service/gitlab-rails/config.ru
git       33779  33107  0 02:39 ?        00:00:01 unicorn worker[1] -D -E production -c /var/opt/gitlab/gitlab-rails/etc/unicorn.rb /opt/gitlab/embedded/service/gitlab-rails/config.ru
git       33782  33107  0 02:39 ?        00:00:01 unicorn worker[2] -D -E production -c /var/opt/gitlab/gitlab-rails/etc/unicorn.rb /opt/gitlab/embedded/service/gitlab-rails/config.ru
root      33802  32528  0 02:39 ?        00:00:00 runsv gitlab-monitor
root      33894  33802  0 02:39 ?        00:00:00 svlogd -tt /var/log/gitlab/gitlab-monitor
root      33974  33914  0 02:40 ?        00:00:00 svlogd -tt /var/log/gitlab/redis-exporter
gitlab-+  34018  32743  0 02:40 ?        00:00:00 postgres: gitlab gitlabhq_production [local] idle
root      34084  34024  0 02:40 ?        00:00:00 svlogd -tt /var/log/gitlab/prometheus
root      34140  34112  0 02:40 ?        00:00:00 svlogd -tt /var/log/gitlab/alertmanager
gitlab-+  34186  32743  0 02:40 ?        00:00:00 postgres: gitlab gitlabhq_production [local] idle
root      34250  34198  0 02:40 ?        00:00:00 svlogd -tt /var/log/gitlab/postgres-exporter
git       34294  33256  0 02:40 ?        00:00:00 /opt/gitlab/embedded/bin/gitlab-workhorse -listenNetwork unix -listenUmask 0 -listenAddr /var/opt/gitlab/gitlab-workhorse/socket -authBackend http://localhost:8080 -authSocket /var/opt/gitlab/gitlab-rails/sockets/gitlab.socket -documentRoot /opt/gitlab/embedded/service/gitlab-rails/public -pprofListenAddr  -prometheusListenAddr localhost:9229 -secretPath /opt/gitlab/embedded/service/gitlab-rails/.gitlab_workhorse_secret -config config.toml
git       34307  33502  0 02:40 ?        00:00:00 /opt/gitlab/embedded/bin/gitaly /var/opt/gitlab/gitaly/config.toml
git       34322  34307  1 02:40 ?        00:00:02 ruby /opt/gitlab/embedded/service/gitaly-ruby/bin/gitaly-ruby 34307 /tmp/gitaly-ruby829523161/socket.0
gitlab-+  34328  33696  0 02:40 ?        00:00:00 /opt/gitlab/embedded/bin/node_exporter --web.listen-address=localhost:9100 --collector.textfile.directory=/var/opt/gitlab/node-exporter/textfile_collector
git       34329  34307  1 02:40 ?        00:00:02 ruby /opt/gitlab/embedded/service/gitaly-ruby/bin/gitaly-ruby 34307 /tmp/gitaly-ruby829523161/socket.1
git       34347  33802  1 02:40 ?        00:00:01 puma 3.12.0 (tcp://localhost:9168) gitlab-monitor]
gitlab-+  34355  33914  0 02:40 ?        00:00:00 /opt/gitlab/embedded/bin/redis_exporter --web.listen-address=localhost:9121 --redis.addr=unix:///var/opt/gitlab/redis/redis.socket
gitlab-+  34371  34024  0 02:40 ?        00:00:00 /opt/gitlab/embedded/bin/prometheus --web.listen-address=localhost:9090 --storage.tsdb.path=/var/opt/gitlab/prometheus/data --config.file=/var/opt/gitlab/prometheus/prometheus.yml
gitlab-+  34478  34112  0 02:40 ?        00:00:00 /opt/gitlab/embedded/bin/alertmanager --web.listen-address=localhost:9093 --storage.path=/var/opt/gitlab/alertmanager/data --config.file=/var/opt/gitlab/alertmanager/alertmanager.yml
gitlab-+  34495  34198  0 02:40 ?        00:00:00 /opt/gitlab/embedded/bin/postgres_exporter --web.listen-address=localhost:9187 --extend.query-path=/var/opt/gitlab/postgres-exporter/queries.yaml
gitlab-+  34500  32743  0 02:40 ?        00:00:00 postgres: gitlab-psql postgres [local] idle
gitlab-+  34526  32743  0 02:40 ?        00:00:00 postgres: gitlab gitlabhq_production [local] idle
gitlab-+  34552  32743  0 02:40 ?        00:00:00 postgres: gitlab gitlabhq_production [local] idle
gitlab-+  34553  32743  0 02:40 ?        00:00:00 postgres: gitlab gitlabhq_production [local] idle
gitlab-+  34554  32743  0 02:40 ?        00:00:00 postgres: gitlab gitlabhq_production [local] idle
gitlab-+  34563  32743  0 02:41 ?        00:00:00 postgres: gitlab gitlabhq_production [local] idle
gitlab-+  34594  32743  0 02:41 ?        00:00:00 postgres: gitlab gitlabhq_production [local] idle
root      34819  29793  0 02:42 pts/0    00:00:00 grep --color=auto gitla


[root@gitlab-01 hmizuno]# ps -ef |grep ^gitla
gitlab-+  32588  32586  0 02:37 ?        00:00:01 /opt/gitlab/embedded/bin/redis-server 127.0.0.1:0
gitlab-+  32743  32741  0 02:38 ?        00:00:00 /opt/gitlab/embedded/bin/postgres -D /var/opt/gitlab/postgresql/data
gitlab-+  32745  32743  0 02:38 ?        00:00:00 postgres: checkpointer process   
gitlab-+  32746  32743  0 02:38 ?        00:00:00 postgres: writer process   
gitlab-+  32747  32743  0 02:38 ?        00:00:00 postgres: wal writer process   
gitlab-+  32748  32743  0 02:38 ?        00:00:00 postgres: autovacuum launcher process   
gitlab-+  32749  32743  0 02:38 ?        00:00:00 postgres: stats collector process   
gitlab-+  33355  33354  0 02:39 ?        00:00:00 nginx: worker process
gitlab-+  33356  33354  0 02:39 ?        00:00:00 nginx: worker process
gitlab-+  33357  33354  0 02:39 ?        00:00:00 nginx: cache manager process
gitlab-+  34018  32743  0 02:40 ?        00:00:00 postgres: gitlab gitlabhq_production [local] idle
gitlab-+  34186  32743  0 02:40 ?        00:00:00 postgres: gitlab gitlabhq_production [local] idle
gitlab-+  34328  33696  0 02:40 ?        00:00:00 /opt/gitlab/embedded/bin/node_exporter --web.listen-address=localhost:9100 --collector.textfile.directory=/var/opt/gitlab/node-exporter/textfile_collector
gitlab-+  34355  33914  0 02:40 ?        00:00:00 /opt/gitlab/embedded/bin/redis_exporter --web.listen-address=localhost:9121 --redis.addr=unix:///var/opt/gitlab/redis/redis.socket
gitlab-+  34371  34024  0 02:40 ?        00:00:00 /opt/gitlab/embedded/bin/prometheus --web.listen-address=localhost:9090 --storage.tsdb.path=/var/opt/gitlab/prometheus/data --config.file=/var/opt/gitlab/prometheus/prometheus.yml
gitlab-+  34478  34112  0 02:40 ?        00:00:00 /opt/gitlab/embedded/bin/alertmanager --web.listen-address=localhost:9093 --storage.path=/var/opt/gitlab/alertmanager/data --config.file=/var/opt/gitlab/alertmanager/alertmanager.yml
gitlab-+  34495  34198  0 02:40 ?        00:00:00 /opt/gitlab/embedded/bin/postgres_exporter --web.listen-address=localhost:9187 --extend.query-path=/var/opt/gitlab/postgres-exporter/queries.yaml
gitlab-+  34500  32743  0 02:40 ?        00:00:00 postgres: gitlab-psql postgres [local] idle
gitlab-+  34526  32743  0 02:40 ?        00:00:00 postgres: gitlab gitlabhq_production [local] idle
gitlab-+  34552  32743  0 02:40 ?        00:00:00 postgres: gitlab gitlabhq_production [local] idle
gitlab-+  34553  32743  0 02:40 ?        00:00:00 postgres: gitlab gitlabhq_production [local] idle
gitlab-+  34554  32743  0 02:40 ?        00:00:00 postgres: gitlab gitlabhq_production [local] idle
gitlab-+  34563  32743  0 02:41 ?        00:00:00 postgres: gitlab gitlabhq_production [local] idle
gitlab-+  34594  32743  0 02:41 ?        00:00:00 postgres: gitlab gitlabhq_production [local] idle


[root@gitlab-01 hmizuno]# ps -ef |grep gitla |grep -v ^gitlab
root       1326      1  0 01:40 ?        00:00:00 /sbin/dhclient -q -lf /var/lib/dhclient/dhclient--eth0.lease -pf /var/run/dhclient-eth0.pid -H gitlab-01 eth0
root      32528      1  0 02:37 ?        00:00:00 runsvdir -P /opt/gitlab/service log: ...........................................................................................................................................................................................................................................................................................................................................................................................................
root      32643  32586  0 02:37 ?        00:00:00 svlogd -tt /var/log/gitlab/redis
root      32848  32741  0 02:38 ?        00:00:00 svlogd -tt /var/log/gitlab/postgresql
git       33089  33087  0 02:39 ?        00:00:00 /bin/bash /opt/gitlab/embedded/bin/gitlab-unicorn-wrapper
git       33107      1 11 02:39 ?        00:00:42 unicorn master -D -E production -c /var/opt/gitlab/gitlab-rails/etc/unicorn.rb /opt/gitlab/embedded/service/gitlab-rails/config.ru
root      33132  33087  0 02:39 ?        00:00:00 svlogd -tt /var/log/gitlab/unicorn
git       33178  33176 12 02:39 ?        00:00:47 sidekiq 5.2.5 gitlab-rails [0 of 25 busy]
root      33201  33176  0 02:39 ?        00:00:00 svlogd -tt /var/log/gitlab/sidekiq
root      33256  32528  0 02:39 ?        00:00:00 runsv gitlab-workhorse
root      33287  33256  0 02:39 ?        00:00:00 svlogd -tt /var/log/gitlab/gitlab-workhorse
root      33354  33352  0 02:39 ?        00:00:00 nginx: master process /opt/gitlab/embedded/sbin/nginx -p /var/opt/gitlab/nginx
root      33369  33352  0 02:39 ?        00:00:00 svlogd -tt /var/log/gitlab/nginx
root      33416  33414  0 02:39 ?        00:00:00 /bin/sh /opt/gitlab/embedded/bin/gitlab-logrotate-wrapper
root      33432  33414  0 02:39 ?        00:00:00 svlogd -tt /var/log/gitlab/logrotate
root      33537  33502  0 02:39 ?        00:00:00 svlogd -tt /var/log/gitlab/gitaly
root      33714  33696  0 02:39 ?        00:00:00 svlogd -tt /var/log/gitlab/node-exporter
git       33776  33107  0 02:39 ?        00:00:01 unicorn worker[0] -D -E production -c /var/opt/gitlab/gitlab-rails/etc/unicorn.rb /opt/gitlab/embedded/service/gitlab-rails/config.ru
git       33779  33107  0 02:39 ?        00:00:01 unicorn worker[1] -D -E production -c /var/opt/gitlab/gitlab-rails/etc/unicorn.rb /opt/gitlab/embedded/service/gitlab-rails/config.ru
git       33782  33107  0 02:39 ?        00:00:01 unicorn worker[2] -D -E production -c /var/opt/gitlab/gitlab-rails/etc/unicorn.rb /opt/gitlab/embedded/service/gitlab-rails/config.ru
root      33802  32528  0 02:39 ?        00:00:00 runsv gitlab-monitor
root      33894  33802  0 02:39 ?        00:00:00 svlogd -tt /var/log/gitlab/gitlab-monitor
root      33974  33914  0 02:40 ?        00:00:00 svlogd -tt /var/log/gitlab/redis-exporter
root      34084  34024  0 02:40 ?        00:00:00 svlogd -tt /var/log/gitlab/prometheus
root      34140  34112  0 02:40 ?        00:00:00 svlogd -tt /var/log/gitlab/alertmanager
root      34250  34198  0 02:40 ?        00:00:00 svlogd -tt /var/log/gitlab/postgres-exporter
git       34294  33256  0 02:40 ?        00:00:00 /opt/gitlab/embedded/bin/gitlab-workhorse -listenNetwork unix -listenUmask 0 -listenAddr /var/opt/gitlab/gitlab-workhorse/socket -authBackend http://localhost:8080 -authSocket /var/opt/gitlab/gitlab-rails/sockets/gitlab.socket -documentRoot /opt/gitlab/embedded/service/gitlab-rails/public -pprofListenAddr  -prometheusListenAddr localhost:9229 -secretPath /opt/gitlab/embedded/service/gitlab-rails/.gitlab_workhorse_secret -config config.toml
git       34307  33502  0 02:40 ?        00:00:00 /opt/gitlab/embedded/bin/gitaly /var/opt/gitlab/gitaly/config.toml
git       34322  34307  0 02:40 ?        00:00:02 ruby /opt/gitlab/embedded/service/gitaly-ruby/bin/gitaly-ruby 34307 /tmp/gitaly-ruby829523161/socket.0
git       34329  34307  0 02:40 ?        00:00:02 ruby /opt/gitlab/embedded/service/gitaly-ruby/bin/gitaly-ruby 34307 /tmp/gitaly-ruby829523161/socket.1
git       34347  33802  1 02:40 ?        00:00:02 puma 3.12.0 (tcp://localhost:9168) [gitlab-monitor]
root      35164  29793  0 02:45 pts/0    00:00:00 grep --color=auto gitla
root      35165  29793  0 02:45 pts/0    00:00:00 grep --color=auto -v ^gitlab
[root@gitlab-01 hmizuno]# 

```