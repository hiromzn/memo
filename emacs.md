## face ( = color )

- How to edit faces

  - M-x list-faces-display

```
(custom-set-faces
 '(button ((t (:inherit link :foreground "cyan"))))
 '(font-lock-comment-face ((t (:foreground "#1076070"))))
 '(link ((t (:foreground "green" :underline t))))
 '(minibuffer-prompt ((t (:foreground "green"))))
 '(tool-bar ((t (:foreground "red" :box (:line-width 1 :style released-button))))))
(custom-set-variables
 '(ansi-color-names-vector ["black" "#d55e00" "#009e73" "#f8ec59" "#0072b2" "#cc79a7" "#56b4e9" "white"])
 '(custom-enabled-themes (quote (deeper-blue))))
```

## environment

- show environment
  ```
  (getenv "HOME")
  ```

# setup

## basic setup for windows

- setup HOME env
  - PC -> property -> システムの詳細設定 -> 詳細設定 -> 環境変数
  - 新規： HOME : <emacs_home_directory>
  - emacsが起動されると、＄HOME/.emacs.d/が作成される。
  - emacsの中で~を使用すれば、HOMEにアクセス可能
      
- ファイルを開くときのデフォルトパスをHOMEに設定する方法
  - emacs.exeのリンクを作成する。
  - 作成したリンクのpropertyの「作業フォルダー」をHOMEに設定する。
	
- Emacs Markdown Mode

  - http://jblevins.org/projects/markdown-mode/

  - Installation : https://stable.melpa.org/#/markdown-mode

  ```
	(require 'package)
	(add-to-list 'package-archives
             '("melpa-stable" . "https://stable.melpa.org/packages/"))
	     (package-initialize)
  ```
  
  - download : https://github.com/jrblevin/markdown-mode

- install w3m
  -    http://w3m.sourceforge.net/
  -    http://emacs-w3m.namazu.org/

    emacs-w3m-1.4.4.tar.gz (2005年3月25日)
    
    D:\Downloads\emacs-w3m-1.4.4>d:\opt\emacs-25.1\bin\emacs.exe -batch -q -no-site-file -l w3mhack.el NONE -f w3mhack-nonunix-install

    (require 'w3m-load)
    
- mode line　：　モードライン

```
     例：　-UUE:----F1

	1文字目: キーボード入力
	2文字目: 画面出力
	3文字目: バッファの文字コード
	4文字目: 行末のコード
	
     文字コード	モードライン	Emacsの設定

     UTF-8	U, u		utf-8-unix
     EUC-JP	E  euc-jp-unix
     JIS	J  iso-2022-jp-unix, junet
     Shift_JIS	S  shift_jis-unix
     Laten 1	1  iso-8859-1-unix


     emacsでの表記 モードライン	制御コード(16進数)	説明	Controlキー	C言語表記
     -unix	   :, (Unix)	0a			nl (new line)		Control+j	\n
     -mac	   (Mac), /	0d			cr (carriage return)	Control+m	\r
     -dos	   (DOS), \	0d 0a (2文字)		cr nl	     Control+m Control+j	\r\n
```
     
- wanderlust : wl
  - http://www.gohome.org/wl/index.ja.html

```
  install:
    get : wl-2.14.-.tar.gz
    see : wl-home/INSTALL.ja

    install : cygwin

    vi WL-MK
      (` (defvar ... ) )
      --> `(defvar ... )
```

## cursor 移動

```
M-f	forward-word	次の単語へ移動
M-b	backward-word	前の単語に移動
M-d	kill-word	単語を削除
C-delete bacward-kill-word	一つ前の単語を削除
M-@	mark-word		前の単語をマーク
M-a	backward-sentence	前の文に移動
M-e	forward-sentence	次の文に移動
M-k	kill-sentence		文を削除
M-z	zap-to-char		指定した文字まで削除 参考
M-SPC	just-one-space		連続したスペースを一つにまとめる
C-M-f	forward-sexp	     次のS式へ移動
C-M-b	backward-sexp	     前のS式へ移動
C-M-n	forward-list	     次の括弧終わりに移動
C-M-p	backward-list	     前の括弧始まりに移動
```

- キーバインディグ確認
  - M-x help-for-help RET b : キーバインドと関数名の対応を調べる。
  - M-x describe-key : (キー入力して) 対応する関数名を調べる。

- sample:
  ```
	(define-key global-map (kbd "C-h") 'delete-backward-char)
	(define-key global-map (kbd "C-r") 'scroll-down)
  ```

- key binding
  - C-v	scroll-up
  - ESC v	scroll-down

- wl
```
  summary
    "T" .... toggle thread view

tab/space
	tabify
	  Command: Convert multiple spaces in region to tabs when possible.
	untabify
	  Command: Convert all tabs in region to multiple spaces, preserving columns.

    tab width
	(setq-default tab-width 2)
	(setq default-tab-width 2)

    M-x set-variable : tab-width : 4
    M-x eval-expression : (setq tab-width 4)

    (setq-default c-basic-offset 4     ;;基本インデント量4
              tab-width 4          ;;タブ幅4
               indent-tabs-mode nil)  ;;インデントをタブでするかスペースでするか
```

- coding system
```
  (set-buffer-file-coding-system 'sjis-unix)

  Ctr-x RETURN f
	sjis-dos
	sjis-unix
	etc...
```

- byte-compile
  - M-x byte-compile-file

  - dired からバイトコンパイルする方法
    - dired でファイルを選び, B を押します
    - 複数のファイルをバイトコンパイルするためには，mでファイレウにマークをつけて，Bを押す

  - M-x byte-recompile-directory
    - 指定したディレクトリでバイトコンパイルが必要なファイルをすべてバイトコンパイル

  - コマンドラインからバイトコンパイル
    - meadow -batch -f batch-byte-compile *.el

```
describe-variable ;; 変数の説明を表示
describe-function ;; 関数の説明を表示
find-function ;; 関数のソースを表示
```

```
PATCH for wl
  wl-util.el
    (wl-parse-addresses
      (skip-chars-forward ",;\t\f\n\r ") <<<< add ";" char.
```

#### magit

- EmacsのGitクライアント

- source

  https://github.com/magit/magit

- install
  - https://magit.vc/manual/magit/Installing-from-an-Elpa-Archive.html#Installing-from-an-Elpa-Archive

  ```
  (require 'package)
  (add-to-list 'package-archives
             '("melpa-stable" . "http://stable.melpa.org/packages/") t)
  ```

  ```
  M-x package-install RET magit RET
  ```
