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
  - PC -> property -> �V�X�e���̏ڍאݒ� -> �ڍאݒ� -> ���ϐ�
  - �V�K�F HOME : <emacs_home_directory>
  - emacs���N�������ƁA��HOME/.emacs.d/���쐬�����B
  - emacs�̒���~���g�p����΁AHOME�ɃA�N�Z�X�\
      
- �t�@�C�����J���Ƃ��̃f�t�H���g�p�X��HOME�ɐݒ肷����@
  - emacs.exe�̃����N���쐬����B
  - �쐬���������N��property�́u��ƃt�H���_�[�v��HOME�ɐݒ肷��B
	
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

    emacs-w3m-1.4.4.tar.gz (2005�N3��25��)
    
    D:\Downloads\emacs-w3m-1.4.4>d:\opt\emacs-25.1\bin\emacs.exe -batch -q -no-site-file -l w3mhack.el NONE -f w3mhack-nonunix-install

    (require 'w3m-load)
    
- mode line�@�F�@���[�h���C��

```
     ��F�@-UUE:----F1

	1������: �L�[�{�[�h����
	2������: ��ʏo��
	3������: �o�b�t�@�̕����R�[�h
	4������: �s���̃R�[�h
	
     �����R�[�h	���[�h���C��	Emacs�̐ݒ�

     UTF-8	U, u		utf-8-unix
     EUC-JP	E  euc-jp-unix
     JIS	J  iso-2022-jp-unix, junet
     Shift_JIS	S  shift_jis-unix
     Laten 1	1  iso-8859-1-unix


     emacs�ł̕\�L ���[�h���C��	����R�[�h(16�i��)	����	Control�L�[	C����\�L
     -unix	   :, (Unix)	0a			nl (new line)		Control+j	\n
     -mac	   (Mac), /	0d			cr (carriage return)	Control+m	\r
     -dos	   (DOS), \	0d 0a (2����)		cr nl	     Control+m Control+j	\r\n
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

## cursor �ړ�

```
M-f	forward-word	���̒P��ֈړ�
M-b	backward-word	�O�̒P��Ɉړ�
M-d	kill-word	�P����폜
C-delete bacward-kill-word	��O�̒P����폜
M-@	mark-word		�O�̒P����}�[�N
M-a	backward-sentence	�O�̕��Ɉړ�
M-e	forward-sentence	���̕��Ɉړ�
M-k	kill-sentence		�����폜
M-z	zap-to-char		�w�肵�������܂ō폜 �Q�l
M-SPC	just-one-space		�A�������X�y�[�X����ɂ܂Ƃ߂�
C-M-f	forward-sexp	     ����S���ֈړ�
C-M-b	backward-sexp	     �O��S���ֈړ�
C-M-n	forward-list	     ���̊��ʏI���Ɉړ�
C-M-p	backward-list	     �O�̊��ʎn�܂�Ɉړ�
```

- �L�[�o�C���f�B�O�m�F
  - M-x help-for-help RET b : �L�[�o�C���h�Ɗ֐����̑Ή��𒲂ׂ�B
  - M-x describe-key : (�L�[���͂���) �Ή�����֐����𒲂ׂ�B

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

    (setq-default c-basic-offset 4     ;;��{�C���f���g��4
              tab-width 4          ;;�^�u��4
               indent-tabs-mode nil)  ;;�C���f���g���^�u�ł��邩�X�y�[�X�ł��邩
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

  - dired ����o�C�g�R���p�C��������@
    - dired �Ńt�@�C����I��, B �������܂�
    - �����̃t�@�C�����o�C�g�R���p�C�����邽�߂ɂ́Cm�Ńt�@�C���E�Ƀ}�[�N�����āCB������

  - M-x byte-recompile-directory
    - �w�肵���f�B���N�g���Ńo�C�g�R���p�C�����K�v�ȃt�@�C�������ׂăo�C�g�R���p�C��

  - �R�}���h���C������o�C�g�R���p�C��
    - meadow -batch -f batch-byte-compile *.el

```
describe-variable ;; �ϐ��̐�����\��
describe-function ;; �֐��̐�����\��
find-function ;; �֐��̃\�[�X��\��
```

```
PATCH for wl
  wl-util.el
    (wl-parse-addresses
      (skip-chars-forward ",;\t\f\n\r ") <<<< add ";" char.
```

#### magit

- Emacs��Git�N���C�A���g

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
