## template

```
sub main
{
	....
}

main( @ARGV );
```

## printf / sprintf

```
printf( "%05d\n", $data );
printf( OUTF "%05d\n", $data );
$out = sprintf( "%05d", $data );
```

## sample base code

```
here document :

print <<~EOF;
  This is a here-doc
  EOF
}
```

```
while(<>)
{
	if ( condition ) next; # next loop (=continue)
	if ( condition ) last; # exit loop (=break)
}
```
```
#!/usr/bin/perl

use File::Basename;

$myfname =  basename( $0 );
$mydir = dirname( $0 );

use Getopt::Long;

my $fname_c = 1;
my $line_c = 2;
my $widerange = 2;
my $topdir = "";
my $debug = 0;

GetOptions(
    'fnamec=s' => \$fname_c,
    'linec=s' => \$line_c,
    'wide=s' => \$widerange,
    'topdir=s' => \$topdir,
    'debug' => \$debug,
    ) || usage() && exit 1;

debug( "program_name: myfname:$myfname, mydir:$mydir\n" );

sub usage()
{
printf( "
usage: $myfname ......
");
}

sub debug()
{
    my( $str ) = @_;
    if ( $debug ) { printf( "debug:$str" ); }
}

```

#### command line options

```
use Getopt::Long;

my $opt_fname = "";
my $opt_linenum = 0;

GetOptions(
    'fname=s' => \$opt_fname,
    'linenum=s' => \$opt_linenum,
    );

# GetOptions
#
#   <NONE> .. flag option (default)
#   =s ...... with argument : ex) -f file_name / -l 123
#   + ....... duplicate option
#   ! ....... arrow NOT

my $opt_all = 0;
my $opt_debug = 0;
my $opt_line = 0;
my $opt_cache = 1;

GetOptions(
    'all' => \$opt_all,
    'debug' => \$opt_debug,
    'line+' => \$opt_line,
    'cache!' => \$opt_cache,
    'fname=s' => \$file_name
    );

print "\$opt_all: $opt_all\n";
print "\$opt_debug: $opt_debug\n";
print "\$opt_line: $opt_line\n";
print "\$opt_cache: $opt_cache\n";

# options : -a
# options : -all
# options : -a -d -l -c
# options : -l -l
# options : -noc
```

#### path

- スクリプトのディレクトリ名の取得

	```
	use FindBin;
	my $script_dir = $FindBin::Bin;
	```

	```
	use File::Basename;
	print "$0\n";
	$path = $0;
	$fn =  basename( $path  );
	$dn = dirname( $path  );
	```

#### パターンマッチ

|code   |comments   |
|---|---|
|if ( 文字列 =~ /パターン/)	| もし「文字列」の中に「パターン」が含まれていれば
|if ( 文字列 !~ /パターン/)	| もし「文字列」の中に「パターン」が含まれていなければ
|if (/パターン/)	| もし変数 $_ の中に「パターン」が含まれていれば
|if (!/パターン/)	|もし変数 $_ の中に「パターン」が含まれていなければ

```
  if ( /pattern/ )
  if ( $_ =~ /pattern/ )

  if ($word =~ /^(...)(.)(.)/) {
    print "$1\n";
    print "$2\n";
    print "$3\n";
  }
```

#### file check

|check   | codes   |
|---|---|
|directory     | if ( -d "dir_name" ) {... |
|書き込み可能　　 | if ( -w "file_name" ) {... |
|read可能　　   | if ( -r "file_name" ) {... |

#### 演算子

  ----------- ---- ------ -----------------------------------------------------
  比較	      数値 文字列 戻り値
  ----------- ---- ------ -----------------------------------------------------
  等しい      ==   eq	  左引数と右引数が等しければ真を返す。
  等しくない  !=   ne	  左引数と右引数が等しくなければ真を返す。
  小さい      <	   lt	  左引数が右引数より小さければ真を返す。
  大きい      >	   gt	  左引数が右引数より大きければ真を返す。
  以下	      <=   le	  左引数が右引数と同じか小さければ真を返す。
  以上	      >=   ge	  左引数が右引数と同じか大きければ真を返す。
  比較	      <=>  cmp	  等しければ0、大きければ1、右引数が大きければ-1を返す。

  文字列一致  =~ !~	  文字列が一致する、しない
  ----------- ---- ------ -----------------------------------------------------

  ------------- ------- -----------------------------
  AND		&&
  OR		||
  ------------- ------- -----------------------------

#### 正規表現


```
[a-zA-Z0-9]	英数字のいずれか1文字
[^a-zA-Z]	英字以外にマッチ
[^0-9]	数字以外にマッチ
```

```
*	直前の文字を0回以上にマッチ
+	直前の文字を1回以上にマッチ
?	直前の文字を0回又は1回にマッチ
{n}	直前の文字をn回にマッチ
{n,}	直前の文字をn回以上にマッチ
{n,m}	直前の文字をn回以上、m回以下にマッチ
```

```
表現	正規表現上の意味
\w	英字、数字、アンダースコア。[a-zA-Z0-9_] に同じ。
\W	英字、数字、アンダースコア以外の文字。[^a-zA-Z0-9_] に同じ。
\d	数字。[0-9] に同じ。
\D	数字以外の文字。[^0-9] に同じ。
\t	タブ
\r	リターン（復帰文字）
\n	改行
\f	ラインフィード（改ページ）
\s	スペース。[\r\t\n\f] に同じ。
\S	スペース以外の文字。[^\r\t\n\f] に同じ。
\a	アラーム（ベル）
\b	バックスペース
\e	エスケープ
\0 + 数字	8進法で表すASCII文字。（ ex. \033, \040 など ）
\x + 英数字	16進法で表すASCII文字。（ ex. \x1b, \x00 など ）
\c[	コントロール文字
\l	次の1文字を小文字にする
\u	次の1文字を大文字にする
\L	\Eまでの文字列を小文字にする
\U	\Eまでの文字列を大文字にする
\E	変更の終わり
\Q	\Eまでの文字列で正規表現のメタ文字を文字にみなす
\b	単語の境界にマッチする
\B	単語の境界以外にマッチする
\A	文字列の最初にマッチする。メタ文字 ^ に同じ。
\Z	文字列の最後にマッチする。メタ文字 $ に同じ。
$ + 数字	グループ化したパターンを後で参照する。( ex. $1, $2, $3, ... )
\ + 数字	上記に同じ。( ex. \1, \2, \3, ... )
$&	マッチした文字列全体
$`	マッチした文字列の前にあるすべての文字列
$'	マッチした文字列の後にあるすべての文字列

```

```
if ( 文字列 =~ /パターン/)	もし「文字列」の中に「パターン」が含まれていれば（パターンマッチすれば真）
if ( 文字列 !~ /パターン/)	もし「文字列」の中に「パターン」が含まれていなければ（パターンマッチすれば偽）

if (/パターン/)	もし変数 $_ の中に「パターン」が含まれていれば（パターンマッチすれば真）
if (!/パターン/)	もし変数 $_ の中に「パターン」が含まれていなければ（パターンマッチすれば偽）
```

```
m/パターン/<修飾子>            :  /パターン/ に同じ
s/パターン/置換文字列/<修飾子>	 : 「パターン」にマッチする文字列を「置換文字列」に置き換える

  ※ /(区切文字)は空白文字以外の記号を利用可能　(例: @, #, *, |, #, {}, [], () など）

修飾子	内容
g	繰り返しマッチする (global)
i	大文字と小文字の区別をしない (case-insensitive)
m	文字列を複数行として扱う (multi-line)
o	変数展開を1度だけ行う (only once)
s	文字列を単一行として扱う (single line)
x	拡張正規表現を行う (extended)
e	置換文字列を「式」と見なす (evaluation)
```
#### 文字列 : string

##### 関数 : string function

```
chomp( $_ );	# 最後の改行の削除
length($str);
```

##### 文字列検索 : string search

```
my $dna = "CATTGTACGTCGCGTAGCCATGTACGTCGCAGT";

my $pos1 = index($dna, "TAC");
print $pos1;
## 5

print substr($dna, $pos1, 3);
## TAC

my $pos2 = rindex($dna, "TAC");
print $pos2;
## 22

print substr($dna, $pos2, 3);
## TAC

print index($dna, "XXX");
## -1
```

##### 文字列のパターンマッチング

```
my $dna = "AGCGAGCCCAAGCTATGCTATTTAGCATTTCGATAAGTCGATGCTAAAAATA";

$dna =~ /(.*)(GCCC.+TAAA)(.*)/;
print $1;
## AGCGA
print $2;
## GCCCAAGCTATGCTATTTAGCATTTCGATAAGTCGATGCTAAA
print $3;
## AATA

print "start position: " . (length($1) - 1);
## start position: 4
print "end position:   " . (length($3) + length($2) + length(3) - 1);
## end position:   47
```
##### 文字列の結合

```
my $dna_1 = "AGCTACGTAGTATT";
my $dna_2 = "ATGCTAGCAAATATATAAAA";
my $dna = $dna_1 . $dna_2;
```

##### 文字列の切り出し

```
my $dna = "AAACCCGGGTTT";
print substr($dna, 0, 3);
## AAA
print substr($dna, 3);
## CCCGGGTTT
print substr($dna, -3);
## TTT

my $dna = "AAACCCGGGTTT";
$dna =~ /AAA(.+)GGG/;
print $1;
## CCC
```

##### 文字列置換

```
my $dna = "AAACCCGGGTTT";
substr($dna, 3, 3, "XXX"); 
print $dna;
## AAAXXXGGGTTT

my $dna = "AAGCAGTXCXGAGCAGXTXAGXTXA";
$dna =~ s/X(.)X/C$1C/g;
print $dna;
## AAGCAGTCCCGAGCAGCTCAGCTCA
```

```
	$str="   Hello test            test   ";
	$str=~ s/\s+//g;
	print $str;

	testという文字列を検索してinabaに置換する

		$str="   Hello test            test   ";
		$str=~ s/(?:test)/inaba/g;
		print $str;
```

##### 文字列分割

```
my $dna = "ACAGTGTATGCCTGCTATGCAGTCGTA";

my @segments = split(/ATG/, $dna);

foreach my $seg (@segments) {
  print $seg, "\n";
}
## ACAGTGT
## CCTGCT
## CAGTCGTA
```

#### 関数定義
	sub myfunc
	{
		(my $arg1, $arg2) = @_;

		or

        ##### arg という名前の変数名は、動作しないので、他の名前にする！　#####
		
		my @args = @_;

		$args[0]...
	}

#### ファイル名と行番号
```
  # filename & line# of input file
  printf( "filename:$ARGV, line#:$.\n" );

  # filename & line# of perl script file
  printf "file[%s] line[%d]\n", __FILE__, __LINE__;

  use File::Basename;
  foo();
  sub foo {
	my ($pkg, $file, $line) = caller;
	$file = basename($file);
	print "ファイル $file の $line 行目から呼び出されました。\n";
  }
```

#### ファイル OPEN
````
  open(INFILE, $file);
  close(INFILE);

  open(OUTFILE, ">$file");
  open(INFILE, "<$file");
  open(PIPEFILE, "|tee log >$file");

  printf( OUTFILE "%d\n", $num );
  
  open(DATAFILE, "< data.txt")) or die("error :$!");

  ### 変数のファイルハンドル
  open( $out, ">OUT" );
  printf( $out "output string\n" );

  ### 配列のファイルハンドル
  open( $out, ">OUT" );
  my @oary = $out;
  printf( { $oary[0] } "output string by array\n" );

  if ( $#ARGV >= 0 ) {
    foreach $file (@ARGV) {
      if ( ! open( INFILE, $file ) ) {
        printf( "ERROR : open file $file\n" );
      } else {
        &func( INFILE, $NOID );
      }
    }
  } else {
      &func( STDIN, $NOID );
  }

sub func
{
  my( $fh, $arg1 ) = @_;

  while( <$fh> ) {
    ...
  }
}
```

#### ファイル読込
```
	while (my $line = <DATAFILE>){
		chomp($line);
		print "$line?n";
		if ( $end_condition ) { last; } # exit from while loop;
	}
```

環境変数
  $home = $ENV{'HOME'};

#### argument of command : 引数

- ARGV
  - 引数が無い場合: $#ARGV は、-1になる。
  - 引数が1つの場合: $#ARGV は、0になる。
  
```
    if ( $#ARGV < 0 ) {
        &usage();
        exit( 1 );
    }
    &process_file();

    foreach $file (@ARGV) {
    }
	
	if (@ARGV == 2){
		my $kokugo = $ARGV[0];
		my $sansuu = $ARGV[1];
	}
	
	shift関数で@ARGVの先頭の引数を受け取る

	    my $file = shift;

	リスト代入を使って、引数を受け取る

	    my ($name, $age) = @ARGV;
		my @nums = @ARGV;

$ cat t.pl
@ary = @ARGV;

sub test
{
printf( "\n\@ary:d:%d\n", @ary );
printf( "\$#ary:%d\n", $#ary );
printf( "ary:list:" ); print @ary;
print "\n";
}

test();

@ary = ( "1" );
test();

@ary = ( "1", "2" );
test();

@ary = ( "1", "2", "3" );
test();

$ perl t.pl

@ary:d:0
$#ary:-1
ary:list:

@ary:d:1
$#ary:0
ary:list:1

@ary:d:1
$#ary:1
ary:list:12

@ary:d:1
$#ary:2
ary:list:123

```

```
	shift
	shift(ARRAY)
	
	配列の先頭の要素を取り出します。要素数は1つ減ります。

	パラメータ:
		ARRAY  対象の配列
	戻り値：
		配列の先頭の要素

	利用方法：
		my @list = ("東京", "大阪");
		my $val = shift(@list);
```

### array

```
my @array = ();
my $array_ref = \@array; # reference of array

my $velue = $array[0];
my $value = $array_ref->[0];

my $size = @array; # size of array
my $last = $#array; # last element number of array
```

### hash

```
my %hash = ();

my %hash = (
   1,11,
   2,22,
   3,33 );
my %hash2 = (
   1=>1111,
   2=>2222,
   3=>3333,
   4=>44 );

my $hash_ref = \%hash; # reference of hash

my $velue = $hash{ $key };
my $value = $hash_ref->{ $key };
```

#### hash of array

```
my $infos = {
  '01:01' => [3, 2.1, 4.6],
  '01:02' => [5, 4.1, 7.4],
  '01:03' => [6, 3.5, 5.7]
};

for my $time (sort keys %$infos) {
  print "$time\n";
  for my $column (@{$infos->{$time}}) {
    print "$column\n";
  }
}
```
#### reference of hash ( map )

```
my %person = (name => 'foo', age => 19);
# or
my $person = {name => 'Ken', age => 19};

my $personref = \%person;

$person2 = %$personref;

my $name  = $personref->{name};
my $age   = $personref->{age};

my $persons = [
  {name => 'Ken',  country => 'Japan', age => 19},
  {name => 'Taro',  country => 'USA', age => 45}
];

for my $person (@$persons) {
  for my $key (keys %$person) {
    my $value = $person->{$key};
    print "$key : $value\n";
  }
}

for my $person (@$persons) {
  for my $key (sort keys %$person) {
    my $value = $person->{$key};
    print "$key : $value\n";
  }
}

for my $person (@$persons) {
  my @rec = (
    $person->{name},
    $person->{country},
    $person->{age}
  );

  print join(',', @rec) . "\n";
}
```

#### reference of array

```
@array = (1, 2, 3);
$arrayref = \@array;

print $arrayref->[0];
print $arrayref->[1];
print $arrayref->[2];

$arrayref2 = [1, 2, 3];

@array2 = @$arrayref;

print $array2[0];
print $array2[1];
print $array2[2];

my @person1 = ('foo', 'JPN', 19);
my @person2 = ('bar', 'USA', 45);

my @persons = (\@person1, \@person2);

my $persons = [
  ['Ken', 'Japan', 19],
  ['Taro', 'USA', 45]
];

$persons->[0]->[0];
$persons->[0]->[1];
# or
$persons->[0][0];
$persons->[0][1];

for my $person (@$persons) {
  for my $column (@$person) {
    print "$column\n";
  }
}

for my $person (@$persons) {
  print join(',', @$person) . "\n";
}

```

#### array : 配列

```
my @array;
my @array = ( "1", "2", "3" );

$number_of_array = $#array + 1;

$array[ 0 ] = "abc";

@words = split( ' ', $line );

foreach $text (@words) {
    printf "text:%s\n", $text;
    printf "XXXX:%s\n", $words2[$idx++];
}

sub dataout {
    local( $date, $time, $ap, $ip, $th, $fr ) = @_;
    local( $hhmm, $a, $b, $c, $d );

    $hhmm = substr( $time, 0, 5 );
    if ( $oldhhmm ne $hhmm ) {
	if ( $olddate ne "OLD" ) {
	    # new
	    $a = $sum_ap/$nrec;
	    $b = $sum_ip/$nrec;
	    $c = $sum_th/$nrec;
	    $d = $sum_fr/$nrec;
	    #print "DATA: $sum_ap $sum_ip $sum_th $sum_fr / $nrec\n";
	    printf( "%s %s %.1f %.1f %.1f %.1f\n", $olddate, $oldhhmm, $a, $b, $c, $d );;

```

##### 連想配列

```
	format :
		my %array;
		$array{ "key" } = "value";

	値の設定：

		$array{ "key1" } = val1;
		$array{ "key2" } = val2;

  %nedan = (
            "bag",        15000,
            "cup",          800,
            "watch",       6000,
            "tv",         38000,
            "camera",     25000
        );

  要素数：
	scalar( keys( %array ) )

  要素list:
	keys( %array )
	
  キーでのソート
    foreach(sort keys %nedan){
      printf "key:%s, val=%6d\n",$_, $nedan{$_};
    }

	foreach( reverse sort keys %nedan){...}

  値でのソート
    foreach(sort {$nedan{$a} <=> $nedan{$b}} keys %nedan){
      printf "%+6s = %6d\n",$_,$nedan{$_};
    }

  内容の表示
    while ( ( $key, $value ) = each( %hash ) ) {
      print "$key : $value\n";
    }

  存在の確認
    if ( exists( $hash{"tempura"} ) ) {
      print "\"tempura\" exists in hash.\n";
    }

    if (defined $fruits{'banana'}) {
       print 'exists';
    }
```

#### 関数

  UPPER CASE
    $upper_case = rc "abc"; --> "ABC"
    $upper_case = rcfirst "abc"; --> "Abc"

  lower case
    $lower_case = lc "ABC"; --> "abc"
    $lower_case = lcfirst "ABC"; --> "aBC"

#### module ( library )

- module ( library ) code

```
package testpkg;  # パッケージ名
 
use Exporter;
@ISA = (Exporter);
@EXPORT = qw(func1 func2);
# exportする関数定義、複数の関数を指定する場合は半角スペースで連結
 
# 変数の定義をローカルに強制（myを付けるように強制）
use strict;
use warnings;

sub func1 {
}

sub func2 {
}
1; # 最後に必ず「真」を返さないといけない

```

- caller code :  呼び出すPerlスクリプト

```
#!/usr/bin/perl
 
use lib qw( ../lib );   # ライブラリパスにモジュールパスを追加
use testpkg;            # モジュールを使用する宣言
 
# 関数呼び出し
my $ret = func2(1, 2, 3);
 
print $ret;
```

- path：実行時指定

```
#!/usr/bin/perl

use File::Basename;

my $dir;

BEGIN {
      $dir = dirname( $0 );
}

use lib "$dir/lib";  # モジュールパスにスクリプト配置パスのlib/ディレクトリを指定
#use lib "$dir";     # モジュールパスにスクリプトと同じDIRを指定
use testpkg;         # モジュールを使用する宣言
 
# 関数呼び出し
my $ret = func2(1, 2, 3);
 
print $ret;
```
