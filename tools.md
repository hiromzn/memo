### tool list for linux

```
$ yum history
ID     | Command line             | Date and time    | Action(s)      | Altered
-------------------------------------------------------------------------------
    44 | install tmux             | 2020-11-05 17:25 | Install        |    2   
    43 | install clang-devel      | 2020-10-22 13:34 | Install        |    1   
    42 | install clang            | 2020-10-22 13:33 | Install        |    3   
    41 | install tcsh             | 2020-10-15 09:35 | Install        |    1   
    40 | install devtoolset-7     | 2020-08-10 10:37 | Install        |   29   
    39 | install devtoolset-9     | 2020-08-08 14:28 | I, U           |   50   
    38 | install centos-release-s | 2020-08-08 14:26 | Install        |    2   
    37 | install scl-utils        | 2020-08-08 14:26 | Install        |    1   
    36 | install gcc-gfortran     | 2020-08-08 10:54 | Install        |    4   
    35 | install yum-utils        | 2020-08-06 15:45 | I, U           |    5   
    34 | install p7zip            | 2020-08-06 15:19 | Install        |    1   
    33 | erase autoconf           | 2020-07-15 11:31 | Erase          |    1   
    32 | install autoconf         | 2020-07-15 10:55 | Install        |    3   
    31 | install gdb              | 2020-07-15 09:44 | Install        |    1   
    30 | install nodejs           | 2020-07-14 15:45 | Install        |    1   
    29 | erase nodejs             | 2020-07-14 15:44 | Erase          |    2   
    28 | install https://rpm.node | 2020-07-14 15:42 | Install        |    1   
    27 | install nodejs           | 2020-07-14 14:30 | Install        |    3   
    26 | install mlocate          | 2020-07-07 17:45 | Install        |    1   
    25 | install libstdc++.i686   | 2020-07-03 23:45 | Install        |    2
```

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
