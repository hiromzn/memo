## url
- https://nodejs.org/ja/

# config

- proxy

```
npm config set proxy http://host:8080
npm config set https-proxy http://host:8080

# npm config set registry http://host:8080
```

# install

## install: centos

```
yum install nodejs
```
## install: mac

- install homebrew

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew -v
```

- install nodebrew

```
brew install nodebrew

nodebrew -v

# show remote version list (which you can install)

nodebrew ls-remote

nodebrew install-binary <version>
nodebrew install-binary latest

mkdir -p ~/.nodebrew/src

nodebrew ls

# set default version you use
nodebrew use <version>

# add path
echo 'export PATH=$HOME/.nodebrew/current/bin:$PATH' >> ~/.bash_profile

```

## multi version

- ndenv
  - https://github.com/riywo/ndenv
  - mac / linux

- nodebrew
  - mac / linux

- nodist
  - windows
  - https://github.com/marcelklehr/nodist
  - usage:
    - nodist -v
    - nodist dist  : show all version
    - nodist + v13.2.0  : install new version
    
### n

- replace all file under /usr/local/

### nvm

- url
  - https://github.com/coreybutler/nvm-windows
- install
  - download ZIP

## package

- https://qiita.com/righteous/items/e5448cb2e7e11ab7d477

- npm
  - パッケージを操作するための CLI(コマンドラインインターフェイス)

- NPM
  - パッケージレジストリ

- パッケージ
  - プログラムのまとまり
  - ライブラリ
  - 利用方法
    - 直接パッケージをダウンロード
    - dependency
      - 依存情報だけを「宣言」して利用する。
  - プロジェクト
    - パッケージと同じ
    - プロジェクトは npm のパッケージ 1 個に対応
  - package.jsonというファイルの親ディレクトリに含まれるファイル群
  - DIRECTORY structure : ~/projects/my-project/
    - package.json
    - /node_modules/
      - 依存パッケージの仮置場としてダウンロードされる場所
  - パッケージの作成
    - initialize : package.jsonを作成する
      - $ npm init
      - 質問をすべてスキップするには-yオプションをつける。

- main
  - パッケージを外からインポートするときにどの JavaScript ファイルが入り口であるかを指定するもの

- dependencies & devDependencies
  - dependencies
    - 実行に必要なパッケージ
  - devDependencies
    - 開発やテストにのみ必要なパッケージ
    である。機能的には、外部のパッケージを 
  - defference :
    - インストールするときにデフォルトではその dependencies はインストールされるが devDependencies はインストールされない

- dependency の編集
  - npm を通じてpackage.jsonを変更する。

  - add dependencies
    - npm install <package>
  - add devDependencies
    - npm install -D <package>

  - specify version : add @version
    - npm install react@16.8.6
  - newest version : add @latest
    - npm install react@latest

  - delete dependency
    - npm uninstall <package>

# 現在のディレクトリに package.json を生成する
npm init
この時点では dependency は何もない。
## install
#### windows
- download & install
- upgrade npm
  - run commands in PowerShell console

```
Set-ExecutionPolicy Unrestricted -Scope CurrentUser -Force
npm install --global --production npm-windows-upgrade
npm-windows-upgrade --npm-version latest
```
#### check version

```
$ node -v
v9.2.0
$ npm --version
5.5.1
```
- npm

```
npm install npm@latest -g

# check
$ npm bin -g
C:\Users\hmizuno\AppData\Roaming\npm

# set path
$ NODE_PATH	%AppData%\npm\node_modules
```

### tips

- ERROR
  - Error: Cannot find module 'D:\work\src\msap\systemcall\init'
- FIX:
  - 
- 

#### xml2json

- https://www.npmjs.com/package/xml2js

- install

```
$ npm install xml2js
```

- sample
```
var parseString = require('xml2js').parseString;
var xml = "<root>Hello xml2js!</root>"
parseString(xml, function (err, result) {
    console.dir(result);
});
```
