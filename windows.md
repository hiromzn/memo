# URL

- shortcuts
  - https://pc-karuma.net/windows-10-keyboard-shortcuts-list/
  
### work space / desctop / screen switch

| key binding     | function |
|-----------------|----------------------------------|
| Win + Ctrl + D  | new work space		|
| Win + Ctrl + F4 | close current desctop |
| Win + D         | switch desctop show |
| Win + Tab       | change work space |
| Ctrl + <mouse wheel> | change desctop icon size |
| Win + Ctrl + -> | change current desctop |
| Win + Ctrl + <- | change current desctop |

### administrator

```
# run as administrator
$ runas /user:administrator command...

# set and show ACL (Access Controle List)
$ cacls file_name
```

### Disk Management

### task scheduler
　コントロールパネル
　　システムとセキュリティ
　　　管理ツール
	タスクスケジューラ

### robocopy
  ---------------------------------------------------------------------------
   ROBOCOPY     ::     Windows の堅牢性の高いファイル コピー
  ---------------------------------------------------------------------------
       簡易な使用法 :: ROBOCOPY コピー元 コピー先 /MIR
           コピー元 :: コピー元ディレクトリ (ドライブ:\パスまたは \\サーバー\パス)。
           コピー先 :: コピー先ディレクトリ (ドライブ:\パスまたは \\サーバー\パス)。
               /MIR :: 完全なディレクトリ ツリーをミラー化します。
	       /MIR はファイルをコピーできるだけでなく、削除もできます。
      詳細な使用方法については、ROBOCOPY /? を実行してください。

http://amaotolog.com/backup/entry188.html
毎日12時にrobocopyでパソコンを自動的にバックアップする方法

how to force foramt
	start admin console ( RightClick[Windows] -> Admin Console)

	C:\WINDOWS\system32>diskpart

	DISKPART> list disk

  	Disk ###  Status         Size     Free     Dyn  Gpt
  	--------  -------------  -------  -------  ---  ---
  	Disk 0    Online          447 GB   446 GB        *
  	Disk 1    Online          238 GB  6144 KB        *

	DISKPART> select disk 0
	Disk 0 is now the selected disk.

	DISKPART> list disk
	  Disk ###  Status         Size     Free     Dyn  Gpt
  --------  -------------  -------  -------  ---  ---
* Disk 0    Online          447 GB   446 GB        *
  Disk 1    Online          238 GB  6144 KB        *

	DISKPART> clean
	DiskPart succeeded in cleaning the disk.
	DISKPART> exit

	Disk Management
		clelect disk & right click -> Initialize disk
			->
Memory dump
	"%SystemRoot%\MEMORY.DMP" 又は "%SystemRoot%\Minidump"
	"Windows" フォルダの直下

Bluetooth マウス　セットアップ

　切れる場合の設定

　１．「コントロールパネル ? デバイスマネージャー」
　　　「Bluetooth ? Bluetooth無線」
　　　「電源の管理」タブ
　　　「電力の節約のために、コンピューターでこのデバイスの電源をオフにできるようにする」のチェックを外す
　　　
　２．「コントロールパネル ? ネットワークと共有センター」
　　　「アダプターの設定の変更」
　　　「Bluetooth ネットワーク接続」を選択して、「このネットワーク デバイスを無効にする」をクリック


(1)スタートアップフォルダ

　スタートアップフォルダにプログラムへのショートカットを保存すれば、
　ユーザログオン時に自動的にプログラムが実行されます。スタートアップフォルダは以下の2種類があります。

	それぞれのユーザ用（そのユーザがログオンした場合のみ実行される）
	すべてのユーザ共通（どのユーザがログオンしても実行される）

　この2種類のスタートアップが合計されて、メニューの[すべてのプログラム][スタートアップ] に表示されます。

　【それぞれのユーザ用】
　　C:\ユーザー\<ユーザ名>\AppData\Roaming\Microsoft\Windows\スタート メニュー\Programs\スタートアップ
　　(英語表示の場合：C:\Users\<ユーザ名>\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup)

　【すべてのユーザ共通】
　　C:\ProgramData\Microsoft\Windows\スタート メニュー\Programs\スタートアップ
　　(英語表示の場合：C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup)

index
	コントロールパネル　→　インデックスのオプション
		→　変更
		→　詳細設定
	OutLook　→　検索（タグ）　→　検索ツール　→　インデックスの状況

chcp
	$ chcp
	$ chcp 932
	$ chcp

	437      IBM437        OEM United States
	932      shift_jis     ANSI/OEM Japanese; Japanese (Shift-JIS)
	1200     utf-16        Unicode UTF-16, little endian byte order (BMP of ISO 10646);
	                       available only to managed applications
	20127    us-ascii      US-ASCII (7-bit)
	20932    EUC-JP        Japanese (JIS 0208-1990 and 0121-1990)
	50220    iso-2022-jp   ISO 2022 Japanese with no halfwidth Katakana;
	                       Japanese (JIS)
	50222    iso-2022-jp   ISO 2022 Japanese JIS X 0201-1989;
	                       Japanese (JIS-Allow 1 byte Kana - SO/SI)
	51932    euc-jp        EUC Japanese
	65001    utf-8         Unicode (UTF-8)

network: netsh : proxy

	>netsh
	netsh>winhttp
	netsh winhttp>show proxy

    IEのプロキシサーバー設定をWindows全体にする

	>netsh
	netsh>winhttp
	netsh winhttp>import proxy ie

hosts files:
	c:\windows\system32\drivers\etc\hosts

	編集するには管理者権限が必要
	
	編集方法
	　管理者権限のコマンドプロンプトを利用
	　メモ帳アイコンを右クリックー＞管理者として実行して編集
	
日付：時刻の表示フォーマットの変更

　Windows7

　コントロールパネル
　　「時計・言語および地域」
　　　地域と言語
　　　　日付、時刻または数値の形式の変更

Windows7: StartUPの設定

　Cドライブ
　└Users
　　└（ユーザー名）
　　　└AppData
　　　　└Roaming
　　　　　└Microsoft
　　　　　　└Windows
　　　　　　　└Start Menu
　　　　　　　　└Programs
　　　　　　　　　└Startup

network
  c:/WINDOWS/system32/drivers/etc/hosts

netstat -ano
  -o option彫ﾋ彫闥､ﾃ彫ﾆ徴ﾗ徴峵･ｻ徴ｹ鎚ﾖ鳥謦､竰ﾉｽ直ｨ調ﾄ追ｽ

  c:\>netstat -ano
  Proto  Local Address          Foreign Address        State           PID
  TCP    0.0.0.0:135            0.0.0.0:0              LISTENING       1088
  TCP    0.0.0.0:445            0.0.0.0:0              LISTENING       4
  TCP    0.0.0.0:1098           0.0.0.0:0              LISTENING       3656

default webブラウザの変更
  コントロールパネル
  --> 「プログラムの追加と削除」
  --> 左の：「プログラムのアクセスと既定の設定」
  --> 「カスタム」
  --> 右のプルダウン
  --> 既定のwebブラウザを選択して下さい。
