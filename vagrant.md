## image URL
http://www.vagrantbox.es/

#### config / files

- ~/.vagrant.d/boxes/
  - vagrantが管理する仮想マシンのISOイメージ置き場所
  - vagrant add したときにダウンロードされるファイル
  - vagrant box listで一覧表示
  - vagrant box remove foo/barで削除
- ~/VirtualBox VMs/
  - VirtualBoxに配備したインスタンスの実体
  - Orace VM Virtualboxマネジャーで一覧表示
  - vagrant destroyで削除

## tips

##### 複数マシン起動

１つのイメージから複数のマシンを起動する方法
- 元となるイメージのダウンロード：
  - vagrant box add bento/centos-6.7

- Vagrantファイルの記述

```
Vagrant.configure("2") do |config|

  # すべてのホストで同じSSH鍵を使う設定
  config.ssh.insert_key = false

  config.vm.define "vagrant0" do |vagrant0|
    vagrant0.vm.box = "bento/centos-6.7"
    vagrant0.vm.network :private_network, ip: "192.168.11.30"
  end

  config.vm.define "vagrant1" do |vagrant1|
    vagrant1.vm.box = "bento/centos-6.7"
    vagrant1.vm.network :private_network, ip: "192.168.11.31"
  end

end
```
- 起動
  `$ vagrant up`

- 停止
    `$ vagrant halt`

- 設定確認　`$ vagrant ssh-config`

- login `$ vagrant ssh [vm_name]`

- 操作
```
$ vagrant box list
```

## windows

#### getting started

**重要**
VirtualBoxマネージャーへのパスを設定しないと、vagrantが利用するプロバイダー(VirtualBox）を見つけることができない。


REF: http://qiita.com/TakashiOshikawa/items/d2fb48d59e9e316af9a2

以下cygwinでのオペレーション：

```
$ eport https_proxy=http://web-proxy.jp.hpecorp.net:8080
$ export http_proxy=http://web-proxy.jp.hpecorp.net:8080
$ vargrant box add  --insecure centos72-64  https://github.com/CommanderK5/packer-centos-template/releases/download/0.7.2/vagrant-centos-7.2.box
$ vagrant box list
centos72-64 (virtualbox, 0)
$ mkdir centos72-1
$ cd centos72-1/
$ vagrant init centos72-1
A `Vagrantfile` has been placed in this directory. You are now
ready to `vagrant up` your first virtual environment! Please read
the comments in the Vagrantfile as well as documentation on
`vagrantup.com` for more information on using Vagrant.

$ vagrant up


```
