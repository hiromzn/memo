# リソースの構造

- resource group
 - subscription ID:
 - virtual machine
    - computer name: TEST01
   - OS: linux
   - size: Basic A0(1core, 0.75GB)
   - public IP address :
   - virtual network :
 - network interfaces : test01484
   - private IP address : 10.0.0.4
   - virtual network : ETST01-vnet
   - public IP address :
   - nsg
   - **接続先** : virtual machine : TEST01
 - storage account : test01diag200
   - performance : Standard
   - replication : LRS : local replication storage
   - BLOB : **boot diagnostics**
     - serial console log :  https://test01diag200.blob.core.windows.net/bootdiagnostics-test01-XXXX.serialconsole.log
     - screenshot :  https://test01diag200.blob.core.windows.net/bootdiagnostics-test01-XXXX.screenshot.bmp
 - storage account : test01disks293
   - performance : Standard
   - replication : LRS : local replication storage
   - BLOB : **vhds**
     - block BLOB : 246 B
     - page BLOB : 29.3 GB
 - public IP address : 13.92.187.149/
 - network security group : TEST01-nsg
 - virtual network : TEST01-vnet


# Azure CLI

### CLI コマンド モード

- Azure CLI には、2 つのコマンド モードが用意されています。
- これら２つのコマンドモードは相互に排他的です。つまり、どちらか一方のモードで作成されたリソースは、他方のモードは管理できません。

  - Azure Resource Manager モード
    - ARMモード
    - Resource Manager デプロイ モデルでの Azure リソースの操作用です。
    - このモードを設定するには、 azure config mode armを実行します。

  - サービス管理モード（Azure  Service Management）
    - ASMモード
    - クラシック デプロイ モデルでの Azure リソースの操作用です。
    - このモードを設定するには、 azure config mode asmを実行します。

- 現在のリリースでは、最初にインストールされたときのモードは Resource Manager モード


### Azure CLI のインストール
<https://docs.microsoft.com/ja-jp/azure/xplat-cli-install>

#### install Azure CLI for RHEL/CnntOS
https://nodejs.org/en/download/package-manager/#enterprise-linux-and-fedora

    # curl --silent --location https://rpm.nodesource.com/setup_6.x | bash -
    # yum -y install nodejs
    # yum install gcc-c++ make

    # npm install -g azure-cli

#### install Azure CLI for windows
https://docs.microsoft.com/ja-jp/azure/xplat-cli-install

オプション 1: npm パッケージのインストール

  最新の Node.js と npm をダウンロードし、インストール
  https://nodejs.org/en/download/package-manager/#windows

  Node.jsのコマンドラインウインドウを開き以下のコマンドを実行
  ＞　npm install -g azure-cli

#### tips

  CLI の更新

     npm update -g azure-cli

  CLIの利用

  Windowsのコマンドラインから以下のコマンドを実行

      ＞ azure

  タブ補完の有効化
  Mac と Linux では、CLI コマンドのタブ補完がサポートされます。
  zsh で有効化する場合は、次のコマンドを実行します。

    echo '. <(azure --completion)' >> .zshrc

  bash で有効化する場合は、次のコマンドを実行します。

      azure --completion >> ~/azure.completion.sh
      echo 'source ~/azure.completion.sh' >> ~/.bash_profile
### Azure CLI から Azure へのログイン
<https://docs.microsoft.com/ja-jp/azure/xplat-cli-connect>

##### シナリオ 1: 対話型ログインによる Azure ログイン

ログインコマンドを実行

    $ azure login

以下のような表示がでるので、

    info:    Executing command login
    info:    To sign in, use a web browser to open the page https://aka.ms/devicelogin and enter the code AMVKJSAKA to authenticate.

ブラウザから　<https://aka.ms/devicelogin>　にアクセスして、コードとして、***AMVKJSAKA*** を入力してデバイスを登録する。すると、以下のようになりログインが成功する。

    \info:    Added subscription 無料試用版
    info:    Added subscription Visual Studio Enterprise
    info:    Setting subscription "無料試用版" as default
    +
    info:    login command OK
### Azure リソース マネージャーの概要
<https://docs.microsoft.com/ja-jp/azure/virtual-machines/azure-cli-arm-commands>

### Linux および Mac での一般的な Azure CLI コマンド
<https://docs.microsoft.com/ja-jp/azure/virtual-machines/virtual-machines-linux-cli-manage>

###### Azure CLI を使用して Azure のリソースとリソース グループを管理する
<https://docs.microsoft.com/ja-jp/azure/xplat-cli-azure-resource-manager>

###### Azure リソース マネージャー テンプレートと Azure CLI を使用した仮想マシンのデプロイと管理
<https://docs.microsoft.com/ja-jp/azure/virtual-machines/virtual-machines-linux-cli-deploy-templates>

### Resource Manager モードでの Azure CLI コマンド
<https://docs.microsoft.com/ja-jp/azure/virtual-machines/azure-cli-arm-commands>

### Azure REST
<https://docs.microsoft.com/ja-jp/rest/api/virtualmachinescalesets/>

### Azure CLI 2.0 (Preview): Command reference - az
<https://docs.microsoft.com/en-us/cli/azure/>

#### az snapshot	Commands to manage snapshots.
<https://docs.microsoft.com/en-us/cli/azure/snapshot>

###### ***Aure Resouce Management modeにする。***

```
$ azure config mode arm
```

###### vm 操作

```
# vm のリスト表示
$ azure vm list
info:    Executing command vm list
info:    Getting virtual machines
data:    ResourceGroupName  Name             ProvisioningState  PowerState  Location  Size
data:    -----------------  ---------------  -----------------  ----------  --------  --------
data:    TEST01             CentOS72-TEST01  Succeeded          VM running  eastus    Basic_A0
info:    vm list command OK

# vm の停止
$ azure vm stop TEST01 CentOS72-TEST01
info:    Executing command vm stop
info:    Looking up the VM "CentOS72-TEST01"
warn:    VM shutdown will not release the compute resources so you will be billed for the compute resources that this Virtual Machine uses.
info:    To release the compute resources use "azure vm deallocate".
info:    Stopping the virtual machine "CentOS72-TEST01"
info:    vm stop command OK

# VM の割り当て解除
$ azure vm deallocate TEST01 CentOS72-TEST01

$ azure vm list
data:    ResourceGroupName  Name             ProvisioningState  PowerState      Location  Size
data:    -----------------  ---------------  -----------------  --------------  --------  --------
data:    TEST01             CentOS72-TEST01  Succeeded          VM deallocated  eastus    Basic_A0

$ azure vm start TEST01 CentOS72-TEST01
info:    Executing command vm start
info:    Looking up the VM "CentOS72-TEST01"
info:    Starting the virtual machine "CentOS72-TEST01"
info:    vm start command OK
```

##### Network Security
```
$ azure -help |grep security
help:      network nic effective-nsg                             Commands to manage effective network security groups
help:      network nsg                                           Commands to manage network security groups
help:      network nsg rule                                      Commands to manage network security group rules
```
###### Network Security group

```
$ azure network nsg list
info:    Executing command network nsg list
info:    Getting the network security groups
data:    Name                 Location  Resource group  Provisioning state  Rules number
data:    -------------------  --------  --------------  ------------------  ------------
data:    CentOS72-TEST01-nsg  eastus    TEST01          Succeeded           7
data:    RHEL72TEST01-nsg     eastus    TEST01          Succeeded           7
data:    TEST01-nsg           eastus    TEST01          Succeeded           7
info:    network nsg list command OK
```
###### Network Security group Rule
```
$ azure network nsg rule list TEST01 CentOS72-TEST01-nsg
info:    Executing command network nsg rule list
info:    Looking up the network security group "CentOS72-TEST01-nsg"
data:    Security rules:
data:    Name                           Source IP          Source Port  Destination IP  Destination Port  Protocol  Direction  Access  Priority
data:    -----------------------------  -----------------  -----------  --------------  ----------------  --------  ---------  ------  --------
data:    default-allow-ssh              *                  *            *               22                TCP       Inbound    Allow   1000
data:    AllowVnetInBound               VirtualNetwork     *            VirtualNetwork  *                 *         Inbound    Allow   65000
data:    AllowAzureLoadBalancerInBound  AzureLoadBalancer  *            *               *                 *         Inbound    Allow   65001
data:    DenyAllInBound                 *                  *            *               *                 *         Inbound    Deny    65500
data:    AllowVnetOutBound              VirtualNetwork     *            VirtualNetwork  *                 *         Outbound   Allow   65000
data:    AllowInternetOutBound          *                  *            Internet        *                 *         Outbound   Allow   65001
data:    DenyAllOutBound                *                  *            *               *                 *         Outbound   Deny    65500
info:    network nsg rule list command OK
```
###### Aapche http用のポートを許可するルールを作成
```
$ azure network nsg rule create TEST01 CentOS72-TEST01-nsg apache-http 100 -p tcp -u 80
info:    Executing command network nsg rule create
warn:    Using default --source-port-range *
warn:    Using default --source-address-prefix *
warn:    Using default --destination-address-prefix *
warn:    Using default --access Allow
warn:    Using default --direction Inbound
info:    Looking up the network security group "CentOS72-TEST01-nsg"
info:    Looking up the network security rule "apache-http"
info:    Creating a network security rule "apache-http"
data:    Id                              : /subscriptions/21636dd2-2449-46df-9023-4a659e830704/resourceGroups/TEST01/providers/Microsoft.Network/networkSecurityGroups/CentOS72-TEST01-nsg/securityRules/apache-http
data:    Name                            : apache-http
data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
data:    Provisioning state              : Succeeded
data:    Source IP                       : *
data:    Source Port                     : *
data:    Destination IP                  : *
data:    Destination Port                : 80
data:    Protocol                        : Tcp
data:    Direction                       : Inbound
data:    Access                          : Allow
data:    Priority                        : 100
info:    network nsg rule create command OK
```
###### Aapche http用のポートを閉じる（Deny)
```
$ azure network nsg rule set TEST01 CentOS72-TEST01-nsg apache-http -c Deny
info:    Executing command network nsg rule set
info:    Looking up the network security group "CentOS72-TEST01-nsg"
info:    Looking up the network security rule "apache-http"
info:    Updating network security rule "apache-http"
data:    Id                              : /subscriptions/21636dd2-2449-46df-9023-4a659e830704/resourceGroups/TEST01/providers/Microsoft.Network/networkSecurityGroups/CentOS72-TEST01-nsg/securityRules/apache-http
data:    Name                            : apache-http
data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
data:    Provisioning state              : Succeeded
data:    Source IP                       : *
data:    Source Port                     : *
data:    Destination IP                  : *
data:    Destination Port                : 80
data:    Protocol                        : Tcp
data:    Direction                       : Inbound
data:    Access                          : Deny
data:    Priority                        : 100
info:    network nsg rule set command OK
```
###### Aapche http用のポートを開ける（Allow)
```
$ azure network nsg rule set TEST01 CentOS72-TEST01-nsg apache-http -c Allow
info:    Executing command network nsg rule set
info:    Looking up the network security group "CentOS72-TEST01-nsg"
info:    Looking up the network security rule "apache-http"
info:    Updating network security rule "apache-http"
data:    Id                              : /subscriptions/21636dd2-2449-46df-9023-4a659e830704/resourceGroups/TEST01/providers/Microsoft.Network/networkSecurityGroups/CentOS72-TEST01-nsg/securityRules/apache-http
data:    Name                            : apache-http
data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
data:    Provisioning state              : Succeeded
data:    Source IP                       : *
data:    Source Port                     : *
data:    Destination IP                  : *
data:    Destination Port                : 80
data:    Protocol                        : Tcp
data:    Direction                       : Inbound
data:    Access                          : Allow
data:    Priority                        : 100
info:    network nsg rule set command OK

```
アカウント環境を管理するコマンド

```
　　account env list [options]
    account env show [options] [environment]
    account env add [options] [environment]
    account env set [options] [environment]
    account env delete [options] [environment]
```

アカウント環境を管理するコマンド

```
　　account env list [options]
    account env show [options] [environment]
    account env add [options] [environment]
    account env set [options] [environment]
    account env delete [options] [environment]
```

```
$ azure config mode arm
info:    Executing command config mode
info:    New mode is arm
info:    config mode command OK

$ azure account list
info:    Executing command account list
data:    Name                      Id                                    Current  State
data:    ------------------------  ------------------------------------  -------  -------
data:    無料試用版                     21636dd2-2449-46df-9023-4a659e830704  true     Enabled
data:    Visual Studio Enterprise  8c3931f1-448f-488a-83f1-ef7d0abbea3a  false    Enabled
info:    account list command OK

$ azure account show
info:    Executing command account show
data:    Name                        : 無料試用版
data:    ID                          : 21636dd2-2449-46df-9023-4a659e830704
data:    State                       : Enabled
data:    Tenant ID                   : 105b2061-b669-4b31-92ac-24d304d195dc
data:    Is Default                  : true
data:    Environment                 : AzureCloud
data:    Has Certificate             : No
data:    Has Access Token            : Yes
data:    User name                   : hiromichi.mizuno@hpe.com
data:
info:    account show command OK

```

#### Azure Backup とは
<https://docs.microsoft.com/ja-jp/azure/backup/backup-introduction-to-azure-backup>

- Azure Backup
 - Microsoft Cloud のデータのバックアップ (または保護) と復元に使用できるAzure ベースのサービス
- コンポーネント (エージェント)
  - コンピューター、サーバー、またはクラウドにダウンロードしてデプロイして利用する。
- ローカル冗長ストレージ (LRS)
  - 同じリージョンのペアリングされているデータセンターにデータが 3 回レプリケートされる。
- geo 冗長ストレージ (GRS)
  - ソース データのプライマリの場所から数百マイル離れたセカンダリ リージョンにデータがレプリケートされる。

##### Azure Backup と Azure Site Recovery
- Azure Backup
  - オンプレミスのデータとクラウドのデータを保護

- Azure Site Recovery
  - 仮想マシンと物理サーバーのレプリケーション、フェールオーバー、フェールバックを実現

#### Resource Manager でデプロイされた VM のバックアップを PowerShell を使用してデプロイおよび管理する
<https://docs.microsoft.com/ja-jp/azure/backup/backup-azure-vms-automation>

- Recovery Services コンテナーを使用すると、Azure Resource Manager でデプロイされた VM と同様に、Azure Service Manager でデプロイされた VM も保護できます。

#### 最初に: Recovery Services コンテナーを使用した Azure VM の保護
<https://docs.microsoft.com/ja-jp/azure/backup/backup-azure-vms-first-look-arm>


## サービス管理 REST API リファレンス
<https://msdn.microsoft.com/ja-jp/library/azure/ee460799.aspx>

更新日: 2015年6月

サービス管理 API を使用すると、管理ポータルを通じて使用できる機能の多くにプログラムでアクセスできます。

サービス管理 API は REST API です。

すべての API 操作は SSL 上で実行され、X.509 v3 証明書を使用して相互認証されます。

### Microsoft Azure サブスクリプション

Microsoft Azure サブスクリプションは、Azure 内で一意のユーザー アカウントです。サービス管理 API を介して使用可能なすべてのリソースは、サブスクリプション下で整理されます。Azure サブスクリプションを作成すると、そのサブスクリプションがサブスクリプション ID によって一意に識別されます。サブスクリプション ID は、サービス管理 API に対して行う各呼び出しの URI の一部になります。

### ストレージ アカウント
ストレージ アカウントは、Azure の BLOB サービス、キュー サービス、およびテーブル サービスの一意のエンドポイントです。BLOB サービス、キュー サービス、およびテーブル サービスの詳細については、「ストレージ サービス REST API リファレンス」を参照してください。

### クラウド サービス
クラウド サービスは Azure のアプリケーション デプロイ用のコンテナーです。クラウド サービスに付ける名前は、Azure 内で一意である必要があります。この名前は、このクラウド サービスを操作するサービス管理 API に対して行う呼び出しの URI の一部を構成します。
サービス管理 API のいくつかの操作では、デプロイをデプロイ名で参照するか、デプロイが実行されているデプロイ環境 (ステージングまたは運用) を参照することで、クラウド サービスを管理できます。
API 操作の一覧については、「クラウド サービスに対する操作」を参照してください。

#### クラウドAPI

    https://msdn.microsoft.com/library/en-us/Ee460812.aspx


# その他の情報
#### 発行設定ファイルの使用

アカウントの発行設定ファイルをダウンロードするには、

    azure config mode asm

と入力して、CLI が Service Management モードであることを確認します。
次に、次のコマンドを実行します。

    azure account download

これにより、既定のブラウザーが開き、 Azure クラシック ポータルにサインインするよう求められます。 サインインした後、.publishsettings ファイルがダウンロードされます。
ファイルを保存した場所をメモしておきます。

- file名
  - 無料試用版-Visual Studio Enterprise-1-31-2017-credentials.publishsettings

ダウンロード ページを使用して選択するか、Azure クラシック ポータルにアクセスして選択すると、選択した Active Directory が、クラシック ポータルおよびダウンロード ページで既定として使用されようになります。 既定を確定した後、'click here to return to the selection page (選択ページに戻るにはここをクリックしてください)' というテキストが、ダウンロード ページの上部に表示されます。 選択されたページに戻るには、用意されているリンクを使用します。

発行設定ファイルをインポートするには、次のコマンドを実行します。

    azure account import <path to your .publishsettings file>

##### 実行結果

    C:\Users\hmizuno>azure config mode asm
	info:    Executing command config mode
	info:    New mode is asm
	info:    config mode command OK

	C:\Users\hmizuno>azure account download
	info:    Executing command account download
	info:    Launching browser to http://go.microsoft.com/fwlink/?LinkId=254432
	help:    Save the downloaded file, then execute the command
	help:      account import <file>
	info:    account download command OK

	C:\Users\hmizuno>azure account import "c:\Users\hmizuno\OneDrive - Hewlett Packard Enterprise\hmizuno\docs\HP\Microsoft-MSDN\azure\無料試用版-Visual Studio Enterprise-1-31-2017-credentials.publishsettings"
	info:    Executing command account import
	info:    account import command OK

##### ダウンロード後のブラウザ画面

- 重要
  - 発行設定をインポートした後は、.publishsettings ファイルを削除する必要があります。
  - このファイルは Azure CLI では不要であり、第三者によってサブスクリプションヘのアクセスに使用されるセキュリティ上のリスクがあるためです。


1 Microsoft Azure プレビュー機能にサインアップする
- 興味のある Microsoft Azure プレビュー機能にサインアップします。
- <http://go.microsoft.com/fwlink/?LinkID=253769&CLCID=0x409>

2 publishSettings ファイルのローカル コピーを保存する
- 警告 このファイルには、エンコードされた管理証明書が含まれています。
- これはサブスクリプションと関連サービスを管理するための資格情報になります。
- このファイルは安全な場所に保存するか、使用後に削除してください。

3 publishSettings ファイルをインポートする
- 次のコマンドを実行

    azure account import

4 新しい Web アプリを作成する
- Git リポジトリで初期化された新しい Web アプリを作成するには、次の Azure PowerShell コマンドを実行します

    azure site create  --git
