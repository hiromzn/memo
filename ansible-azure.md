# 準備

-----

## Ansible用インスタンスの準備

### リソースグループ作成

### 仮想ネットワーク作成

### NSG作成

sshを許可する

### パブリックIP作成

Staticにする

### 仮想マシン作成

-----

## Ansibleのインストール

### ログイン

作成した仮想マシンインスタンスにsshでログイン

### 依存パッケージのインストール

```
$ sudo yum check-update
$ sudo yum install -y gcc libffi-devel python-devel openssl-devel
$ sudo yum groupinstall -y "Development Tools"
```


### Ansibleインストール

```
$ sudo yum install -y epel-release
$ sudo yum install -y sshpass
$ sudo yum install -y ansible
```

### バージョン確認

```
$ ansible --version
ansible 2.2.1.0
  config file = /etc/ansible/ansible.cfg
  configured module search path = Default w/o overrides
```

-----

## Azure CLI

### Azure CLIのインストール

https://docs.microsoft.com/en-us/cli/azure/install-azure-cli

azure-cliをインストール

```
$ curl -L https://aka.ms/InstallAzureCli | bash
```

Shellを再起動

```
$ exec -l $SHELL
```




### Azure CLIからのログイン有効化
```
$ az login
```

表示に従って、ブラウザでhttps://aka.ms/devicelogin を開いてコードを入力する。

### 確認

```
$ az account list
```
複数のサブスクリプションが表示される場合は
```
$ azure account set {{サブスクリプションID}}
```

-----

## Ansible準備

### pipのインストール
```
curl -kL https://bootstrap.pypa.io/get-pip.py | sudo python
```
### Azure Python SDKのインストール

https://docs.microsoft.com/ja-jp/azure/python-how-to-install

```
$ sudo pip install --pre azure
```

-----

## Azure準備

### 認証情報の設定

`Service Principal`を使う。

#### Azure CLIを使う場合

##### AD Applicationの作成

```
$ az ad app create --display-name "ansible-app" --homepage "http://dummy/ansible" --identifier-uris "http://dummy/ansible" --password AnsibleAppPassword
```

##### サービスプリンシパルの作成

IDの確認

```
$ az ad app show --id http://dummy/ansible
{
  "appId": "ec3e93e4-7fa5-46fe-ae19-f603c3a15a9b",
  "appPermissions": null,
  "availableToOtherTenants": false,
  "displayName": "ansible-app",
  "homepage": "http://dummy/ansible",
  "identifierUris": [
    "http://dummy/ansible"
  ],
  "objectId": "17b98b34-e6ea-47f6-a66f-549516d379e5",
  "objectType": "Application",
  "replyUrls": []
}
```
サービスプリンシパルの作成
```
$ az ad sp create --id  ec3e93e4-7fa5-46fe-ae19-f603c3a15a9b
{
  "appId": "ec3e93e4-7fa5-46fe-ae19-f603c3a15a9b",
  "displayName": "ansible-app",
  "objectId": "f796353c-af24-490d-a57b-1cf91c34a997",
  "objectType": "ServicePrincipal",
  "servicePrincipalNames": [
    "ec3e93e4-7fa5-46fe-ae19-f603c3a15a9b",
    "http://dummy/ansible"
  ]
}
```

##### サブスクリプションのOwner権限を付与

```
$ az account show
{
  "environmentName": "AzureCloud",
  "id": "b1709c01-73d2-4535-a3c9-b041ef4980bb",
  "isDefault": true,
  "name": "Visual Studio Enterprise",
  "state": "Enabled",
  "tenantId": "640da3bc-6c66-4090-b4fa-0d6859327b71",
  "user": {
    "name": "junichi.yoshise@hpe.com",
    "type": "user"
  }
}
```
```
$ az role assignment create --assignee http://dummy/ansible --role "Owner" --scope /subscriptions/b1709c01-73d2-4535-a3c9-b041ef4980bb
{
  "id": "/subscriptions/b1709c01-73d2-4535-a3c9-b041ef4980bb/providers/Microsoft.Authorization/roleAssignments/62843ce9-d838-4b44-a0e2-5ab896de3726",
  "name": "62843ce9-d838-4b44-a0e2-5ab896de3726",
  "properties": {
    "principalId": "f796353c-af24-490d-a57b-1cf91c34a997",
    "roleDefinitionId": "/subscriptions/b1709c01-73d2-4535-a3c9-b041ef4980bb/providers/Microsoft.Authorization/roleDefinitions/8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
    "scope": "/subscriptions/b1709c01-73d2-4535-a3c9-b041ef4980bb"
  },
  "type": "Microsoft.Authorization/roleAssignments"
}
```

#### Azureポータルから行う場合

[ポータルでサービスプリンシパルを作成する](https://docs.microsoft.com/ja-jp/azure/azure-resource-manager/resource-group-create-service-principal-portal)

### Azure接続情報ファイルの設置
```
ansible@junichi-mint ~
$ vi ~/.azure/credentials
ansible@junichi-mint ~
$ cat ~/.azure/credentials
[default]
subscription_id=b1709c01-73d2-4535-a3c9-b041ef4980bb
tenant=640da3bc-6c66-4090-b4fa-0d6859327b71
client_id=ec3e93e4-7fa5-46fe-ae19-f603c3a15a9b
secret=AnsibleAppPassword
```

-----

## Ansibleから接続確認

```
$ ansible localhost -m azure_rm_resourcegroup_facts
[WARNING]: provided hosts list is empty, only localhost is available

localhost | SUCCESS => {
   "ansible_facts": {
       "azure_resourcegroups": [
           {
               "location": "japaneast",
               "name": "Jenkins",
               "properties": {}
           },
           {
               "location": "japaneast",
               "name": "jyoshise-EARTH-study",
               "properties": {}
           },
           {
               "location": "japaneast",
               "name": "stackato-demo",
               "properties": {}
           }
       ]
   },
   "changed": false
}
```
