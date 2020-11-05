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
  prefix + ?	�L�[�o�C���h�ꗗ
  prefix + s	�Z�b�V�����̈ꗗ�\��
  prefix + c	�V�K�E�B���h�E�쐬�E�ǉ�
  prefix + w	�E�B���h�E�̈ꗗ
  prefix + &	�E�C���h�E�̔j��
  prefix + n	���̃E�C���h�E�ֈړ�
  prefix + p	�O�̃E�C���h�E�ֈړ�
  prefix + |	���E�Ƀy�C������
  prefix + -	�㉺�Ƀy�C������
  prefix + h	���̃y�C���Ɉړ�
  prefix + j	���̃y�C���Ɉړ�
  prefix + k	��̃y�C���Ɉړ�
  prefix + l	�E�̃y�C���Ɉړ�
  prefix + H	�y�C�������Ƀ��T�C�Y
  prefix + J	�y�C�������Ƀ��T�C�Y
  prefix + K	�y�C������Ƀ��T�C�Y
  prefix + L	�y�C�����E�Ƀ��T�C�Y
  prefix + x	�y�C���̔j��
  prefix + �X�y�[�X	�y�C���̃��C�A�E�g�ύX
  prefix + Ctrl + o	�y�C���̓���ւ�
  prefix + {	�y�C���̓���ւ�(�����)
  prefix + }	�y�C���̓���ւ�(������)
  prefix + [	�R�s�[���[�h�̊J�n(�J�[�\���L�[�ňړ�)
  prefix + v	�R�s�[�J�n�ʒu����(vi���[�h)
  prefix + y	�R�s�[�I���ʒu����(vi���[�h)
  prefix + Crtl + p	�R�s�[���e�̓\��t��
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
- �N�Z��PDF

### benchmark tool
- hdbench clone

### find command
- ```
	find / \( -user foo -o -name \*oya\* \) -printf "%b %p\n" \
      |awk '{ if ($1 > 8192) print $2; }' |xargs rm -rf;

    %b ..... file block size.
	```
### ieHTTPHeaders
- IE�ɑg�ݍ���ŁA�ڑ���T�[�o�[�̃z�X�g���AReferer�ACookie�̓��e�Ȃǂ��m�F�\

### valgring
- linux memory leak check tool

### libevent (http://monkey.org/~provos/libevent/)
- The libevent API provides a mechanism to execute a callback function
	when a specific event occurs on a file descriptor or after a timeout
	has been reached. Furthermore, libevent also support callbacks
	due to signals or regular timeouts.
