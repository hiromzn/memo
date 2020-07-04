## initial setup

- VBoxGuestAdditions
  - Virtual box のウィンドウに [デバイス] > [Guest Additions CD イメージの挿入] を選択
  - 「"VBox_GAs_5.2.8"には自動的に起動するソフトウェアが含まれています。実行しますか？」->[実行する] を選択
- [仮想マシン]ー＞「一般」ー＞「高度」ー＞（クリップボードの共有、ドラッグ＆ドロップ）＝双方向
- Network
  - VirtualBoxにホストオンリーネットワークを追加
		- [VirtualBox] - [環境設定] から[ネットワーク]タブを選択。
		- 右側にある「＋」ボタンを押下。
 
  - ゲストOSをReBootする。
    - ネットワークを有効にするため。
  
	- ゲストOSにアダプタを設定
		- VirtualBoxマネージャーからゲストOSの[設定]タブを選択。
		- [ネットワーク]タブを選択。
		- アダプタ１に「NAT」を設定。
		- アダプタ２に「ホストオンリーネットワーク」を設定。
  
#### network

- NAT　
	- アドレス変換。ゲストOS・インターネット間の通信ができる。
	- ホストOSからゲストOSへアクセスできない。

- ブリッジアダプタ
	- ホスト端末が複数NICを備えている場合に、一つのNICをゲストOSに割り当てる。

- 内部ネットワーク
	- ゲストOS「同士」で通信できるようになる。
	- ホストOSからゲストOSへはアクセス出来ない。

- ホストオンリーアダプタ
	- ホストOS・ゲストOS間で通信可能。
	- ゲストOSからインターネットには出られない。

- ゲストOSにNATとホストオンリーアダプタの両方を設定する

	- ゲストOSがインターネットに出ていく時はNATアダプタで通信
	- ホストOSと通信する時はホストオンリーアダプタで通信

	- VirtualBoxにホストオンリーネットワークを追加
		- [VirtualBox] - [環境設定] から[ネットワーク]タブを選択。
		- 右側にある「＋」ボタンを押下。

	- ゲストOSにアダプタを設定
		- VirtualBoxマネージャーからゲストOSの[設定]タブを選択。
		- [ネットワーク]タブを選択。
		- アダプタ１に「NAT」を設定。
		- アダプタ２に「ホストオンリーネットワーク」を設定。

#### share
  mount point : /media/sf_<share_name>

## TIPS

- resize FIXED fize disk

  - copy image
    - VBoxManage.exe clonedhd "xxx.vdi" "new_xxx.vdi"
    	- VBoxManage.exe clonehd "D:\virtualbox\vm\CentOS_75-DISK30GFIX\CentOS_75-DISK30GFIX.vdi" "D:\virtualbox\vm\CentOS_75-DISK30GFIX\CentOS_75-VariableDISK.vdi"
	
  - change type from fix to variable.
    - select -> settinsg -> strage -> remove & add new *.vdi
    
  - resize storage size
    - VBoxManage.exe modifyhd --resieze <size> "new_xxx.vdi"

  - resize guest disk partition
    - centOS 
      - yum install gparted
      - sudo gparted
        - resize partition

  - extend(resize) CentOS file system
    - show_information
      - # vgdisplay
    - extend
      - # lvextend -l +100%FREE /dev/centos/root
      - # xfs_growfs /dev/centos/root 

- Virtual box のウィンドウリサイズ

  - 仮想マシンを起動する。
  - Virtual box のウィンドウに [デバイス] > [Guest Additions CD イメージの挿入] を選択する。
  - 「"VBox_GAs_5.2.8"には自動的に起動するソフトウェアが含まれています。実行しますか？」とダイアログが表示されるので、[実行する] を選択する。
  - 管理者パスワードを要求されるので入力して [認証する] を選択する。
  - 処理が終わると、「Press Return to close this window...」と表示されるので、[Enter] キーをクリックする。
  - 仮想マシンを再起動する。

  - <補足>
    - 仮想マシンの [設定] > [ストレージ] の [コントローラ IDE] に "VBoxGuestAdditionsiso" が自動的に追加され、設定後は取り外しても問題ない。
    - （ちなみに、この iso ファイルは "C:\Program Files\Oracle\VirtualBox\" 内にある。）

- Install Guest Additions
  ```
  # mount -r /dev/cdrom /mnt/cdrom/

  # yum install -y bzip2 gcc make kernel-devel kernel-headers dkms gcc-c++
   or
  # yum groupinstall "Deveopment Tools"
  # yum install kernel-devel kernel-headers

  # sh /mnt/cdrom/VBoxLinuxAdditions.run
  ```

## TROUBLE

- 問題
  - 「Install Guest Addsinions」ー＞「マウントできません（could not mount media）」エラー
　　http://pc-karuma.net/mac-virtualbox-fullscreen-mode/

- 解決方法
　- ゲストOSをシャットダウン。
　- Virtual Box マネージャの「設定」→「ストレージ」を開き、
　- 右側パネルの「コントローラ：IDE」→「VBoxGuestAdditions.iso」を選択。
　- さらに右パネルの「属性」→「CD/DVDドライブ」の右側にあるボタンを押し、ドロップダウン表示の中から「仮想ドライブからディスクを除去」をクリック。

　- ゲストOSを再起動
　- 「VirtualBoxのメニューから「Devices」→「Install Guest Addsinions」を指定すると、（先ほどのエラーが解消されて）ダイアログが現れる。


- 問題
  - vagrant up時に「mount: unknown filesystem type 'vboxsf'」が発生する
  - http://berukann.hatenablog.jp/entry/2016/01/24/163124


- 原因
  - VirtualBox Guest Additions」がインストールされていなかったのが原因
  - GuestOS内に「Virtualbox Guest Additions」がない、もしくはバージョンが違う
  - 実際にログインしてみてもコマンドがない
    ```
	  vagrant@vagrant-ubuntu-trusty:/usr/bin$ vboxsf
	  vboxsf: command not found
    ```

- 解決策

```
  策１) GuestOS内に「Virtualbox Guest Additions」を手動インストールする

  VirtualBoxのバージョンのGuestAdditionsのISOを探す
	http://download.virtualbox.org/virtualbox/
	http://download.virtualbox.org/virtualbox/5.0.14/VBoxGuestAdditions_5.0.14.iso

  「Virtualbox Guest Additions」を手動インストールする

  GUEST:
  $ wget http://download.virtualbox.org/virtualbox/5.0.14/VBoxGuestAdditions_5.0.14.iso
  $ sudo mkdir /media/VBoxGuestAdditions
  $ sudo mount -o loop,ro VBoxGuestAdditions_5.0.14.iso /media/VBoxGuestAdditions
  $ sudo sh /media/VBoxGuestAdditions/VBoxLinuxAdditions.run
  $ rm VBoxGuestAdditions_5.0.14.iso
  $ sudo umount /media/VBoxGuestAdditions
  $ sudo rmdir /media/VBoxGuestAdditions
  $ exit

  HOST:
  % vagrant reload
```
