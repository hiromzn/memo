## URL

- Python (本家)
  https://www.python.org/

- 日本Pythonユーザ会
  http://www.python.jp/

- docs
  - https://docs.python.org/3/
  - japanese : https://docs.python.jp/

- Python入門
  - http://www.tohoho-web.com/python/

## install

- Windowsの場合

```
# msys
$ pacman -S python
```

  Windowsの場合、http://www.python.org/ から [Downloads] → [Windows] を選び、アーキテクチャに応じたインストーラ (例: python-2.7.9.amd64.msi) をダウンロードしてインストールしてください。

- Linux(Red Hat / CentOS)

  ```
  sudo yum -y install python
  ```

- CentOS 7.0 / Python 3.6 / 3.8

  ```
  $ sudo yum install rh-python38
  $ sudo yum install rh-python38-python-pip
  $ sudo yum install rh-python38-python-pysocks

  $ sudo scl enable rh-python38 bash
  # env |grep proxy
  https_proxy=socks5h://localhost:3127
  # pip install pandas
  ```

  ```
  $ sudo yum install rh-python36.x86_64
  $ vi ~/.bashrc
  . /opt/rh/rh-python36/enable
    or
  $ .  /opt/rh/rh-python36/enable
  ```

- Linux(Ubuntu / Debian)の場合
  ```
  sudo apt-get install python2.7
  ```

- install with source code

  - CentOS 7.0 / Python 2.7.9 with make

    ```
    # yum -y install wget gcc zlib-devel gdbm-devel readline-devel
    # yum -y install sqlite-devel openssl-devel tk-devel bzip2-devel

    $ wget https://www.python.org/ftp/python/2.7.9/Python-2.7.9.tgz
    $ tar zxvf Python-2.7.9.tgz
    $ cd Python-2.7.9
    $ ./configure --with-threads --enable-shared --prefix=/usr/local
    $ make

    # sudo make altinstall
    ```

## pip

- windows
```
# msys
$ pacman -S python3-pip
```

- INSTALL of Linux

```
curl -O https://bootstrap.pypa.io/get-pip.py >get-pip.py
python get-pip.py
#
# ==> pythonコマンドのバージョンのpython用にpipがインストールされる。
# REF: https://pip.pypa.io/en/stable/installing/#installing-with-get-pip-py
#
# python get-pip.py --user # install to the user site
#
pip --version

# CentOS 7
yum install epel-release
yum install python2-pip
pip install -U pip
```

- upgrade(UPDATE)

```
pip install -U package_name
pip install --upgrade package_name
```

## interpreter mode

```
$ phthon
>>> 1 + 1
2
```

## script

```
#!/usr/bin/env python
# -*- coding: cp-1252 -*-
```
## command line arguments :

```
import sys
print "This is the name of the script: ", sys.argv[0]
print "Number of arguments: ", len(sys.argv)
print "The arguments are: " , str(sys.argv)
```

```
import argparse

#
# parse arguments
#
parser = argparse.ArgumentParser(description="calculate X to the power of Y")

# exclusive options
group = parser.add_mutually_exclusive_group()
group.add_argument("-v", "--verbose", action="store_true")
group.add_argument("-q", "--quiet", action="store_true")

# option with value
parser.add_argument("-z", type=int, default=1, help="(x**y) * z")

# arguments
parser.add_argument("x", type=int, help="the base")
parser.add_argument("y", type=int, help="the exponent")

# parse !
args = parser.parse_args()

#
# main program
#

# get answer
answer = ( args.x ** args.y ) * args.z

# print answer
if args.quiet:
    print(answer)
elif args.verbose:
    print("{} to the power {} X {} equals {}".format(args.x, args.y, args.z, answer))
else:
    print("{}^{} * {} == {}".format(args.x, args.y, args.z, answer))

```
```
$ ./parse_args.py
usage: parse_args.py [-h] [-v | -q] [-z Z] x y
parse_args.py: error: the following arguments are required: x, y

$ ./parse_args.py -h
usage: parse_args.py [-h] [-v | -q] [-z Z] x y

calculate X to the power of Y

positional arguments:
  x              the base
  y              the exponent

optional arguments:
  -h, --help     show this help message and exit
  -v, --verbose
  -q, --quiet
  -z Z           (x**y) * z
```


## variable: 変数

|name | comment |
|-----|---------|
| _ | 直前の値 |

#### list : tuple(変更不可) : dictionary : enum

```
list:リスト	foo = ['tom', 'mike', 'nancy', 'jenny', 'jack']
tuple:タプル	foo = ('tom', 'mike', 'nancy', 'jenny', 'jack')
dic:辞書	foo = {'tom': 20, 'mike': 21, 'nancy': 'unknown', 'jenny': 12 'jack': 55}
enum:集合	foo = set('tom', 'mike', 'nancy', 'jenny', 'jack')
```

##### list

- standard operations
```
list1 = [ 'a', 'b', 'c' ]
list2 = list1 + [ 'd' ]
```

##### リスト操作関数: function for list
```
append( val )	リストの末尾に1つの要素を追加
extend( arr )	リストの末尾に配列（複数要素）を追加
insert( index, val )	リストの指定したインデックスに要素を追加
del( index )   インデックスで指定したリストの要素を削除
remove( val )  リストから指定した値をもつ要素を削除
pop()	要素のポップ
index( val )	リストから指定した値をもつ要素のインデックスを取得
count( val )	リスト内で指定した値をもつ要素の数を取得
join( arr )	リストの文字列への変換（split関数の逆）
sort( reverse= True/False )	リストをソートする（数値なら昇順,文字列ならアルファベット順）
len( arr )     リスト内要素数を取得
```

```
sample:

list_sample = ["hoge", "huga", "hage"]

# 配列の要素を1つずつ表示
for x in list_sample:
    print(x)  
```

##### 辞書操作関数: function for dictionary

```
関数	意味
update( dict )	辞書の結合
del( key )   指定したkeyをもつ要素の削除
clear( ) 全要素削除

# sample code:
dict_sample = {"red": 100, "green": 0, "blue": 200}
dict_sample2 = {"cyan": 50, "magenta": 60, "yellow": 70, "key_plate": 80}

# 辞書の結合
dict_sample.update(dict_sample2)

# 要素の追加
dict_sample["black"] = 123

# 上書き : keyが重複しているため
dict_sample["red"] = 123  

dict_sample = {"red": 100, "green": 0, "blue": 200}

# 辞書のkeyを1つずつ表示
for k in dict_sample.keys():
    print(k)  

# 辞書のvalueを1つずつ表示
for v in dict_sample.values():
    print(v)  

# 辞書のkey,value両方を表示
for k, v in dict_sample.items():
    print(k + ":" + str(v))  

```

##### 集合操作関数 : function for enum
```
関数	演算	意味	例
set1.union( set2 )	set1 | set2	和集合	{1,2,3} | {2,4,6} ⇒ {1, 2, 3, 4, 6}
set1.intersection( set2 )    set1 & set2	積集合（共通集合）   {1,2,3} & {2,4,6} ⇒ {2}
set1.difference( set2 )	set1 - set2 差集合	{1,2,3} - {2,4,6} ⇒ {1, 3}
set1.symmetric_difference( set2 )   set1 ^ set2	排他的OR（どちらか片方のみに属する）	{1,2,3} ^ {2,4,6} ⇒ {1, 3, 4, 6}
```
#### local vs global : scope

```
id=0

def func():
  global id # specify global
  id += 1
  print id

func()
```

## 型: type

|name      | comment |
|----------|---------|
| int	   |  |
| float    |  |
| Decimal  |  |
| Fraction |  |
| j / J    | 複素数 |

### 演算子 :

```
+=	# 例（ a+=1 ）
-=	# 例（ a-=1 ）
```

### 文字列 : string

- standard operations
```
mystr = 'ABC'
mystr2 = mystr + 'DEF'
```

- byte / str

  - strからbytes : encodeでbytes型(utf-8)に変換します。

```
>>> 'あいう'.encode('utf-8')
b'\xe3\x81\x82\xe3\x81\x84\xe3\x81\x86'    #utf8のバイト列
```

  - UTF-8 bytesからstr : decodeで文字列に変換します。


>>> b'\xe3\x81\x82\xe3\x81\x84\xe3\x81\x86'.decode('utf-8')
'あいう'
```

```
'spam eggs'  # single quotes
"spam eggs"  # double quotes
'doesn\'t'     	  # \' to escape the single quote
"\"Yes,\" he said."	  # \" to escape the double quote

a = 'a' + 'b'

```

- match

```
import re

m = re.match(r"FIRE[0-9]+", "FIRE123")
if m:
    print("matchした")
else:
    print("matchしてない")
```

- replace

```
src = 'I like orange.'
dst = src.replace('orange', 'apple') # 'I like apple.'

import re
src = 'I like orange.'
dst = re.sub(r'[a-z]+', 'xxx', src) # 'I xxx xxx.'
```

- 置き換え (str.translate)

```
import string
src = 'I like orange.'
dst = src.translate(string.maketrans('abcde', 'ABCDE')) # 'I likE orAngE.'
```

- strip

```
>>> str='  string str2          '
>>> str
'  string str2  \t'

>>> str.strip()
'string str2'

>>> str.rstrip()
'  string str2'

>>> str.lstrip()
'string str2  \t'
```

- split

```
>>> list="a,b,c,d"
>>> list.split()
['a,b,c,d']

>>> list.split(',')
['a', 'b', 'c', 'd']
>>> list.split(',', 2)
['a', 'b', 'c,d']

>>> list.rsplit(',')
['a', 'b', 'c', 'd']
>>> list.rsplit(',', 2)
['a,b', 'c', 'd']
```

```
##
## file extention
##

# check file extention

if file_name.endswith('.mp3'):
   ...
elif file_name.endswith('.flac'):
   ...

# m.lower().endswith(('.png', '.jpg', '.jpeg'))

# get file extention

filepaths = ["/folder/soundfile.mp3", "folder1/folder/soundfile.flac"]
for fp in filepaths:
    # Split the extension from the path and normalise it to lowercase.
    ext = os.path.splitext(fp)[-1].lower()
    ...

```
##### escape sequence

```
\改行 : バックスラッシュと改行が無視される
\\ : バックスラッシュ(\)
\' : シングルクォート(')
\" : ダブルクォート(")
\a : ベル(BEL)
\b : バックスペース(BS)
\f : フォームフィード(FF)
\n : 改行(LF)
\r : 復帰(CR)
\t : タブ(TAB)
\v : 垂直タブ(VT)
\nnn : 8進表記文字(nは0～7)
\xnn : 16進表記文字(nは0～f)
\uxxxx : ユニコード文字xxxx (例: u"\u3042")
\U....xxxx : ユニコード文字xxxxxxxx (例: U"\U00003042")
\N{name} : Unicodeデータベース文字 (例: u"\N{HIRAGANA LETTER A}")
```

##### 文字列のフォーマット : string format
```
%s 文字列
%d 整数
%f 浮動小数点数
%x 16進数
%o 8進数
%% %自身

print "%s" % "ABC"          #=> ABC
print "%d" % 123            #=> 123
print "%f" % 1.23           #=> 1.23
print "%x" % 255            #=> ff
print "%o" % 255            #=> 377
print "%%%d" % 80           #=> %80

# tips
print( line, end='' ) # 最終文字をNULLに指定し改行を削除

##
## print format
##
#### %
s = 'alice'
i = 25
print('%s is %d years old' % (s, i))


#### format
print('Alice is {} years old'.format(i))
print('{} is {} years old'.format(s, i))

print('{0} is {1} years old / {0}{0}{0}'.format(s, i))
# Alice is 25 years old / AliceAliceAlice
print('{name} is {age} years old'.format(name=s, age=i))

# here document

string1 = '''
  This is a {what}.
  I'm from {where}.
'''.format(what="apple", where="Chiba").strip()
print(string1)

import textwrap
string2 = textwrap.dedent('''
  This is a {what}.
  I'm from {where}.
main()
{{ // <<<< double !!!!
  statementes;
}} // <<<< double !!!!
''').format(what="apple", where="Chiba").strip()
print(string2)

#### f'....'
print(f'{s} is {i} years old')
# Alice is 25 years old
```

###### Unicode 文字列
- format : u'xxxxx'
- code : \uXXXX ==> 0xXXXX(unicode)
- raw モード: 文字列を開始するクオート文字の前に ‘ur’

##### unicode sample
```
>>> u'Hello World !'
u'Hello World !'
>>> u'Hello\u0020World !'
u'Hello World !'
```

##### row mode sample
```
>>> ur'Hello\u0020World !'
u'Hello World !'
>>> ur'Hello\\u0020World !'
u'Hello\\\\u0020World !'
```

###### 複数行の文字列 : string, multi line
```
print """\
Usage: thingy [OPTIONS]
     -h                        Display this usage message
     -H hostname               Hostname to connect to
"""
```
###### 結合（＋）: string, append
```
>>> 3 * 'un' + 'ium'
'unununium'
```

###### 文字列のインデックス : string, index

- 指定：　[n], [m:n], [n:], [:n]
  - first position : 0
  - [x:] : include x
  - [x:y] : include x, exclude y
    - [0:3] = { 0,1,2 }

- 文字数：　len(word)
- Python の文字列は変更できない。
  - 文字列のインデクスで指定したある場所に代入を行うとエラーが発生

```
>>> word = 'Python'
>>> word[0]  # character in position 0
'P'
>>> word[5]  # character in position 5
'n'
>>> word[-1]  # last character
'n'
>>> word[-2]  # second-last character
'o'

>>> word[0:2]  # characters from position 0 (included) to 2 (excluded)
'Py'
>>> word[2:5]  # characters from position 2 (included) to 5 (excluded)
'tho'

# 開始インデクスは常に含まれ、終了インデクスは常に含まれない
# s[:i] + s[i:] が常に s と等しい

>>> word[:2] + word[2:]
'Python'
>>> word[:4] + word[4:]
'Python'
>>> word[:2]   # character from the beginning to position 2 (excluded)
'Py'
>>> word[4:]   # characters from position 4 (included) to the end
'on'
>>> word[-2:]  # characters from the second-last (included) to the end
'on'

>>> len(s)
34

```

#### immutable : 変更不能体

```
>>> word = 'Python'
>>> word[0]
'P'
>>> word[0] = 'J'
...
TypeError: 'str' object does not support item assignment

# how to create new string
>>> word2 = 'J' + word[1:]
>>> word2
'Jython'
```

#### list vs tuple

- tuple : タプル
  - 変更ができないオブジェクト
  - 「イミュータブル」（ immutable ）
  - tuple1 = ('東京都', '神奈川県', '大阪府')
  - check type
    - type(tuple1) == tuple  # => True
  - convert to tuple
    - tuple( list )  # listをtuple型に変換

- list : リスト
  - 変更できるオブジェクト
  - 「ミュータブル」（ mutable ）

```
someTuple = (1,2)
someList  = [1,2]

>>> t = (1,2)
>>> l = [1,2]
>>> t
(1, 2)
>>> l
[1, 2]
>>> tuple( l )
(1, 2)
```

#### list

```
#
# get item by matching string
#
some_list = ['abc-123', 'def-456', 'ghi-789', 'abc-456']
if any("abc" in s for s in some_list):
    # whatever

matching = [s for s in some_list if "abc" in s]
```

```
>>> seq=[1,2,3,4,5]
>>> seq
[1, 2, 3, 4, 5]
>>> seq[0]
1
>>> seq[9]
...
IndexError: list index out of range
>>> seq[4]
5
>>> seq[-1]
5
>>> seq[-2]
4
>>> seq[-2:]
[4, 5]
>>> seq[1:]
[2, 3, 4, 5]
>>> seq[:3]
[1, 2, 3]

# 連結
>>> seq + [ 10, 20, 30 ]
[1, 2, 3, 4, 5, 10, 20, 30]

###
### list is "mutable" : 変更可能体
###
>>> seq[3]
4
>>> seq[3] = 444
>>> seq
[1, 2, 3, 444, 5]

### 要素の追加
>>> seq.append(999)
>>> seq
[1, 2, 3, 444, 5, 999]

>>> seq[2:4]
[3, 444]

### list要素の置き換え
>>> seq[2:4] = [ 30, 40 ]
>>> seq
[1, 2, 30, 40, 5, 999]

### list要素の削除
>>> seq[2:4] = []
>>> seq
[1, 2, 5, 999]
>>> seq[:] = []
>>> seq
[]

```

## function

```
# standard function
def f(x):
    """print x * x""""
    ret = x ** 2
    return ret

print( f(2 ) )

# default value for argument
def f2(x, y=2):
    return x ** y

print( f2( 2, 3 ) )
print( f2( 2 ) )

# keyword argument
def prkey( **keywords ):
    keys = sorted( keywords.keys() )
    for kw in keys:
    	print( kw, ":", keywords[kw] )

prkey( key3="333", key1="111", key2="two" )

```

```
#
# get function name
#
import traceback
def get_func_name():
   stack = traceback.extract_stack()
   filename, codeline, funcName, text = stack[-2]
   return funcName

#
# get function name
#
import inspect
import logging
import traceback

def get_function_name():
    return traceback.extract_stack(None, 2)[0][2]

def get_function_parameters_and_values():
    frame = inspect.currentframe().f_back
    args, _, _, values = inspect.getargvalues(frame)
    return ([(i, values[i]) for i in args])

def my_func(a, b, c=None):
    logging.info('Running ' + get_function_name() + '(' + str(get_function_parameters_and_values()) +')')
    pass

logger = logging.getLogger()
handler = logging.StreamHandler()
formatter = logging.Formatter(
    '%(asctime)s [%(levelname)s] -> %(message)s')
handler.setFormatter(formatter)
logger.addHandler(handler)
logger.setLevel(logging.INFO)

my_func(1, 3) # 2016-03-25 17:16:06,927 [INFO] -> Running my_func([('a', 1), ('b', 3), ('c', None)])
```

## exit
```
exit

import sys
sys.exit( 0 )
sys.exit( 1 )
sys.exit( "ERROR" )   # exit code is 1
```   

## environment

```
import os

# get env
print(os.environ.get('LANG'))
print(os.environ.get('NEW_KEY'))
print(os.environ.get('NEW_KEY', 'default'))

# add env
os.environ['NEW_KEY'] = 'test'

# delete env
del os.environ['NEW_KEY']

print(os.environ.pop('NEW_KEY'))
print(os.environ.pop('NEW_KEY'))	# -> KeyError: 'NEW_KEY'
print(os.environ.pop('NEW_KEY', None)) 	# -> None
```

## exec : run : call

```
import subprocess

subprocess.call( 'ls' )
subprocess.call( ( "ls", "-l", "/tmp/" ) )
subprocess.call( 'ls -al /tmp', shell=True )
```

```
try:
    res = subprocess.check_output('ls')
except:
    print "Error."
print res

args = ['ls', '-l', '-a']
try:
    res = subprocess.check_call(args)
except:
    print "Error."

cmds = ['gcc', '-c', 'test.c']
try:
    res = subprocess.run(
    	cmds,
    	check=True,
        stdout=subprocess.PIPE,
	stderr=subprocess.STDOUT, # cmds 2>&1
	# stderr=subprocess.PIPE, # cmds >stdout 2>stderr
        universal_newlines=True)
    print( '# stdout/stderr: ' );
    for line in result.stdout.splitlines():
    	print( line )
except subprocess.CalledProcessError:
    print('ERROR: run command [' + cmd + ']', file=sys.stderr)

>>> import subprocess
>>> subprocess.run(['ls','-l','/dev/null'])
crw-rw-rw-  1 root  wheel    3,   2  2 12 20:05 /dev/null
CompletedProcess(args=['ls', '-l', '/dev/null'], returncode=0)
>>> subprocess.run(['ls','-l','/dev/null'], stdout=subprocess.PIPE)
CompletedProcess(args=['ls', '-l', '/dev/null'], returncode=0, stdout=b'crw-rw-rw-  1 root  wheel    3,   2  2 12 20:29 /dev/null\n')
>>> subprocess.run(['ls','-l','/dev/null'], stdout=subprocess.PIPE, stderr=subprocess.PIPE)
CompletedProcess(args=['ls', '-l', '/dev/null'], returncode=0, stdout=b'crw-rw-rw-  1 root  wheel    3,   2  2 12 20:29 /dev/null\n', stderr=b'')
>>> subprocess.run(['ls','-l','/dev/nullX'], stdout=subprocess.PIPE, stderr=subprocess.PIPE)
CompletedProcess(args=['ls', '-l', '/dev/nullX'], returncode=1, stdout=b'', stderr=b'ls: /dev/nullX: No such file or directory\n')

##
import subprocess
import sys

res = subprocess.run(["ls", "-l", "-a"], stdout=subprocess.PIPE)
sys.stdout.buffer.write(res.stdout)

## PIPE sample

import subprocess
from subprocess import PIPE

print( '##### echo XXX |sort' )
with subprocess.Popen('sort', shell=True, stdin=PIPE, stdout=PIPE, stderr=PIPE,
        universal_newlines=True) as pipe:
    out, err = pipe.communicate('One\nTwo\nThree\nFour')
    for line in out.splitlines():
        print('===', line)

print( '##### ls |sort' )
ls_result = subprocess.run('ls', shell=True, check=True,
        stdout=PIPE, universal_newlines=True)
sort_result = subprocess.run('sort', shell=True, check=True,
        input=ls_result.stdout, stdout=PIPE, universal_newlines=True)
for line in sort_result.stdout.splitlines():
    print('===', line)

print( '##### cat pipe.py |sort' )
with open('pipe.py', encoding='utf-8') as file:
    proc = subprocess.run('sort', shell=True, check=True,
            stdin=file, stdout=subprocess.PIPE, universal_newlines=True)
    for line in proc.stdout.splitlines():
        print('===', line)
```

## 制御構造

#### while

```
a, b = 0, 1
while b < 10:
  print b
  a, b = b, a+b

```

#### if

```
# check file type
import os
os.path.isfile( "/path/bob.txt" )
os.path.isdir( "/path/bob" )
```

```
>>> if x < 0:
...     x = 0
...     print( "to zero" )
... elif x == 0:
...     print( "zero" )
... elif x == 1:
...     print( 'one' )
... else:
...     print( 'more' )
...
more

if x < 0 and y > 10:
if x < 0 or y > 10:
if x < 0 xor y > 10:
```


## file IO

```
#! python3

f = open( 'filename', 'r' )
f = open( 'filename', 'w' )
f = open( 'filename', 'rb+' )

data = f.read( size )
line = f.readline()

for line in f:
    print( line )
    print( line, end='' ) # 最終文字をNULLに指定し改行を削除

```

## with

```
print( '#### read with.py' )
with open("with.py", "r") as f:
    print(f.read())

print( '#### read with.py -> write with.py.backup~' )
with open("with.py", "r") as fr:
    with open("with.py.backup~", "w") as fw:
        fw.write(fr.read())

print( '#### class sample' )
class MySampleClass:

  def __enter__(self):
      print('START')
      return self

  def myfunc(self):
      print('Do something...')

  def __exit__(self, exception_type, exception_value, traceback):
    print('END')

with MySampleClass() as c:
  c.myfunc()
```
