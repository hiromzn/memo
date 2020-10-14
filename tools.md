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
