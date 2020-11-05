### tmux

- terminal multiplexer
- URL : https://github.com/tmux/tmux/wiki
- how to use
  ```
  $ tmux
  ```

- commands
  - prefix : Ctrl+b (default)

  ```
  prefix + ?	キーバインド一覧
  prefix + s	セッションの一覧表示
  prefix + c	新規ウィンドウ作成・追加
  prefix + w	ウィンドウの一覧
  prefix + &	ウインドウの破棄
  prefix + n	次のウインドウへ移動
  prefix + p	前のウインドウへ移動
  prefix + |	左右にペイン分割
  prefix + -	上下にペイン分割
  prefix + h	左のペインに移動
  prefix + j	下のペインに移動
  prefix + k	上のペインに移動
  prefix + l	右のペインに移動
  prefix + H	ペインを左にリサイズ
  prefix + J	ペインを下にリサイズ
  prefix + K	ペインを上にリサイズ
  prefix + L	ペインを右にリサイズ
  prefix + x	ペインの破棄
  prefix + スペース	ペインのレイアウト変更
  prefix + Ctrl + o	ペインの入れ替え
  prefix + {	ペインの入れ替え(上方向)
  prefix + }	ペインの入れ替え(下方向)
  prefix + [	コピーモードの開始(カーソルキーで移動)
  prefix + v	コピー開始位置決定(viモード)
  prefix + y	コピー終了位置決定(viモード)
  prefix + Crtl + p	コピー内容の貼り付け
  ```

### cscope

- https://github.com/tsuyopon/memo/blob/master/DEVTOOLS/Cscope.md
- tag jump tool for C / C++ / Java / PHP4
- has plugin for vim

### gnu global
- https://www.gnu.org/software/global/

- basic operation
  
### sql migrate
- https://github.com/rubenv/sql-migrate
	- Atomic migrations
	- Supports SQLite, PostgreSQL, MySQL, MSSQL and Oracle databases

### PDF tool
- CutePDF
- クセロPDF

### benchmark tool
- hdbench clone

### find command
- ```
	find / \( -user foo -o -name \*oya\* \) -printf "%b %p\n" \
      |awk '{ if ($1 > 8192) print $2; }' |xargs rm -rf;

    %b ..... file block size.
	```
### ieHTTPHeaders
- IEに組み込んで、接続先サーバーのホスト名、Referer、Cookieの内容などを確認可能

### valgring
- linux memory leak check tool

### libevent (http://monkey.org/~provos/libevent/)
- The libevent API provides a mechanism to execute a callback function
	when a specific event occurs on a file descriptor or after a timeout
	has been reached. Furthermore, libevent also support callbacks
	due to signals or regular timeouts.
