# reference

## UNIXネットワークプログラミング

- http://cms.phys.s.u-tokyo.ac.jp/~naoki/CIPINTRO/NETWORK/index.html

### Network byte order, big endian と littel endian

- http://cms.phys.s.u-tokyo.ac.jp/~naoki/CIPINTRO/NETWORK/endian.html

- ネットワークのバイトオーダーは、big endian
  - windows, linuxは、Little endian
  - Solaris, HPUX, AUXは、big endian
  
- よって、4バイト長のIPアドレスと、2バイト長のポート番号は、バイトオーダーチェックおよび必要に応じてエンディアンの変換が必要

- 自動変換関数（変換が必要な場合に変換する関数）
  - htonl : long型のデータをhostのバイトオーダーからNetwork byte orderに変換
  - htons : short型のデータをhostのバイトオーダーからNetwork byte orderに変換
  - ntohl : long型のデータをNetwork byte orderからhostのバイトオーダーに変換
  - ntohs : short型のデータをNetwork byte orderからhostのバイトオーダーに変換

- sample:
  - unsigned long ip = htonl(0x7f000001);
    - htonl関数は long型の値を host でのメモリ扱い方法を network の世界での方法 (network byte order = big endian) に、必要があれば、変換する。
    - よって、どのマシンでも自動的に big endian として 変数に値を代入できる。

  - unsigned short port = htons(80);
    - short型の値であるポート番号 big endian に統一する。
  
  - printf("IP = %08x PORT=%u", ntohl(ip), ntohs(port) );
    - ntohl, ntohs : network order を host order に戻す。

### UNIXネットワークプログラミングに登場する構造体

- http://cms.phys.s.u-tokyo.ac.jp/~naoki/CIPINTRO/NETWORK/struct.html

- summary of network structure

  - sockaddr
    - ソケットを利用するための情報
    - sockaddr型は汎用型
　- sockaddr_un
    - sockaddr型のUNIX LOCAL用に特化した情報
  - sockaddr_in
    - sockaddr型のInternet用に特化した情報
  - sockaddr_in
    - Internet用ソケットのアドレスの情報
    - defined in netinet/in.h
  - in_addr
    - IPアドレスを記述
    - defined in netinet/in.h
  - hostent
    - マシンのIPアドレスなどの情報を調べる際に使う
    - defined in netdb.h

- struct sockaddr
  - プロセスが持つソケットにプロセス間通信ができるように OS上のアドレスを割り当てるための手続き書類を作成するための sockaddr型構造体
  - sys/socket.h 内で定義

```
struct sockaddr {
	u_char		sa_len;		/* total length */
	sa_family_t	sa_family;	/* address family */
	char		sa_data[14];	/* actually longer; address value */
};
```

  - sockaddr型は汎用型であり、実際はこれを UNIX LOCAL用に特化した sockaddr_un型と Internet用に特化した sockaddr_in型 を使う。

- sockaddr_un
  - UNIX LOCAL なソケットのアドレスの情報
  - sys/un.h 内で定義


```
struct	sockaddr_un {
	u_char	sun_len;		/* sockaddr len including null */
	u_char	sun_family;		/* AF_UNIX */
	char	sun_path[104];		/* path name (gag) */
};
```

  - この変数を用いてUNIX LOCALなソケットのアドレスを指定したり、逆にソケットの アドレスを調べたりする。
  - アドレスを指定する時
    - 第2メンバ sun_family には #define 定数 AF_LOCAL の値を代入
    - 第3メンバ sun_path にアドレスの代わりとなる UNIX domain ソケット と呼ばれるファイルの名前を代入
  - アドレスを調べる時
    - 調べられた結果がこの変数に入る。
    - 第1メンバ sun_len にはこの変数のサイズが代入
    - sun_family には AF_LOCAL
    - sun_path には用いられているアドレスの UNIX domain ソケットの ファイル名が代入
  - 余談: sun_family には昔は AF_UNIX の#define定数を代入していた

  - sockaddr_un型変数の"正しい"設定の仕方は以下

```
sockaddr_un addr;

bzero( &addr, sizeof(addr) );
addr.sun_family = AF_LOCAL;
strcpy( addr.sun_path, "/tmp/localudp" );
```

  - sockaddr_un型変数のMACROを使った設定

```
sockaddr_un addr = SOCKADDR_UN_INIT( AF_LOCAL, "/tmp/localudp" );
```

- sockaddr_in and in_addr

```
struct sockaddr_in {
	u_char	sin_len;
	u_char	sin_family;
	u_short	sin_port;
	struct	in_addr sin_addr;
	char	sin_zero[8];
};

struct in_addr {
	u_int32_t s_addr;
};
```

  - この変数を用いてInternet用のソケットのアドレスを指定したり、逆にソケットの アドレスを調べたりする。
  - アドレス指定
    - 第2メンバ sin_family には #define 定数 AF_INET の値を代入
    - 第3メンバ sin_port にマシン内でのアドレスの代わりのようであるポートの値を network byte orderで代入する。
    - 第4メンバ sin_addr
      - in_addr型構造体
      	- 1メンバだけを持つ構造体
	- s_addr にはマシンのIPアドレスを network byte orderで代入する。
  - アドレスを調べる時
    - 第1メンバ sin_len にはこの変数のサイズが代入
    - sin_family には AF_INET
    - sin_port, sin_addr にはポートとIPアドレスが代入されています。

  - sockaddr_in型変数の"正しい"設定

```
sockaddr_in addr;

bzero( &addr, sizeof(addr) );

addr.sin_family = AF_INET;
addr.sin_port   = htons(80);
inet_aton( "127.0.0.1", &addr.sin_addr );
```

  - sockaddr_in型変数のMACRO設定

```
sockaddr_in addr = SOCKADDR_IN_INIT( AF_INET, htons(80), InAddr("127.0.0.1")) );
```

- hostent
  - マシンのIPアドレスなどの情報を調べる際に使う
  - netdb.h 内で定義

```
struct	hostent {
	char	*h_name;	/* official name of host */
	char	**h_aliases;	/* alias list */
	int	h_addrtype;	/* host address type */
	int	h_length;	/* length of address */
	char	**h_addr_list;	/* list of addresses from name server */
#define	h_addr	h_addr_list[0]	/* address, for backward compatibility */
};
```

  - h_name
    - マシンの正式名称
  - h_aliases[i]
    - マシンの別名（存在すれば入る）
    - このポインタ配列の最後の要素は NULL になっている
  - h_addr_list[i]
    - マシンが持ついくつかのIPアドレスが入る。
    - 普通は0番目の要素しか使わないので、h_addr という #define マクロが用意されている。
    - h_addr は char* 型である。
      - IPアドレスはこのアドレスに 文字列として入っているのではなく、h_addr[0], h_addr[1], h_addr[2], h_addr[3] の計 4つのchar型変数に整数値として入っている。
      - これを in_addr型構造体の変数に 代入するには正しくは bcopy関数を使う
        - *(in_addr*) h_addr とする キャスト変換でも可能

  - hostent型変数の"正しい"設定の仕方は以下のとおりです。

```
// program hostent.cc
#include <stdio.h>
#include <string.h>
#include <netdb.h>
#include "integnet.h"

int main( int, char* argv[] )
{
  hostent* hp;
  if( (hp = gethostbyname( argv[1] )) == NULL ){
    herror("gethostbyname");
    return 0;
  }

  printf("h_name         = %s\n", hp->h_name );
  printf("h_addrtype     = %d\n", hp->h_addrtype );
  printf("h_length       = %d\n", hp->h_length );

  int i;
  for( i=0; hp->h_aliases[i]; i++ ){
    printf("h_aliases[%d]   = %s\n", i, hp->h_aliases[i] );
  }
  for( i=0; hp->h_addr_list[i]; i++ ){
    in_addr sin_addr;
    bcopy( hp->h_addr_list[i], &sin_addr, hp->h_length );

    printf("h_addr_list[%d] = %s\n", i, inet_ntoa(sin_addr) );
  }
  return 0;
}
```