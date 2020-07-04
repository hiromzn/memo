#### Trouble shoot tips

- 起動できない。

  - safe modeで立ち上げる。
  ```
  $ atom --safe
  ```

- console
  - menu: 表示ー＞開発ー＞デベロッパーツール
  - select [console]TAB
  - input
    - > process.env.PATH
    - > process.env.LANG

#### HOME
- ATOMのHOMEを変更する方法
  - ATOM_HOME環境変数を指定する。
  - しかし、AppData\Roaming　フォルダーのパスは変更されないので、USERPROFILEで指定する必要がある。
  - その場合、USERPROFILEで指定したフォルダにAppData\Roamingを作成する必要がある。

#### packages

##### terminal-plus
- how to install
  atomのsettingsからインストールするとプロンプトが表示されないことがあるので、apmを使ってインストールする

    $ apm install LarsKumbier/terminal-plus

##### mini-*

### インストール

#### package
- git-plus  
- japanese-menu  
- emacs-plus
- disable-keybindings
- auto-encoding
- monokai
- advanced-open-file
- (disable) remove-all-keybindings

##### **git-plus インストール時の注意事項**

- gitコマンドへのパスを通す必要あり。
  - 環境変数のPATHにCygwinのパスを通す。

    `PATH=....d:\cygwin\bin`

- PATH環境変数追加方法

  - atom[Welcome Guide] -> "Hack on the Init Script"
  - push[Open your Init Script]
  - 以下のスクリプトを.atom/init.coffeeに追加する。

    ```
    process.env.PATH = [
      "D:\\cygwin\\bin"
      process.env.PATH
    ].join(";");
    ```

##### emacs-plus
- need :
  - disable-keybindings
  - keymap.cson add following config


        '.editor':
          'ctrl-a': 'editor:move-to-first-character-of-line'
          'ctrl-e': 'editor:move-to-end-of-line'
          'ctrl-j': 'editor:backspace-to-beginning-of-word'
          'ctrl-l': 'emacs:recenter'
          'ctrl-h': 'core:backspace'
          'ctrl-r': 'core:page-up'
          'ctrl-d': 'core:delete'
          'ctrl-/': 'core:undo'

        '.tree-view':
          'ctrl-b': 'tree-view:collapse-directory'
          'ctrl-f': 'tree-view:expand-item'

        '.workspace':
          # cursor
          'ctrl-p': 'core:move-up'
          'ctrl-n': 'core:move-down'
          'ctrl-b': 'core:move-left'
          'ctrl-f': 'core:move-right'

#### keybind

- config
  - keymap.cson

- ctrl-.
  - Key Binding Resolver

##### emacs keybind
- <http://qiita.com/onegear0o/items/2958255fba1125a72489>
- Atom @Windowsで Emacs風 に快適に動かすためにしたこと。 Emacs keybindins の実現。 ctrl-kの削除。
- 全部のkeybindingsを削除するのには、以下二つのpackagesをinstall

 - File > settings > Packages
 - remove-all-bindings https://atom.io/packages/remove-all-keybindings
 - disable-keybindings https://atom.io/packages/disable-keybindings

#### code  

- encodingの選択:
  - Ctrl + Shift + u

#### proxy
- set
  ```
  $ npm config set https-proxy http://proxy-server.example.com:10080
  $ npm config set https-proxy http://username:password@proxy-server.example.com:10080
  ```
  - Firewall のなかには，セキュリティ上の理由から， SSL/TLS 暗号通信を中間者攻撃1 でのぞき見するものがある。 このタイプの Firewall/Proxy は SSL/TLS の証明書を書き換えてしまうため， apm が通信エラーになる。 この場合は以下の設定を行って強制的に SSL/TLS を通すようにする
    ```
    $ npm config set strict-ssl false
    ```

- unset
  ```
  $ npm config delete https-proxy
  ```

- get
    ```
    $ npm config get https-proxy
    ```

# command
文字コードの設定：　Ctrl-Shift-U

mdファイルのプレビュー：　Ctrl-Shift-m

### emacs keybind

#### <http://qiita.com/onegear0o/items/2958255fba1125a72489>

Atom @Windowsで Emacs風 に快適に動かすためにしたこと。 Emacs keybindins の実現。 ctrl-kの削除。

- 全部のkeybindingsを削除するのには、以下二つのpackagesをinstallしました。

 - File > settings > Packages
 - remove-all-bindings https://atom.io/packages/remove-all-keybindings
 - disable-keybindings https://atom.io/packages/disable-keybindings
