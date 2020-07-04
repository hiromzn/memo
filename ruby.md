# tools

- gem

```
$ gem help
RubyGems is a sophisticated package manager for Ruby.
```

- rake

```
Rakeとは Rubyで記述されたビルドツール

install
$ gem install rake
```

# install

- CentOS

  - os bundle version install
```
sudo yum install ruby
```

  - newer version install
```
#-----------------------------
# install packages
#-----------------------------
sudo yum update
sudo yum install -y git
sudo yum install -y bzip2 gcc openssl-devel readline-devel zlib-devel

#-----------------------------
# install rbenv / ruby-bundle
#-----------------------------
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
git clone git://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
# edit profile
vi ~/.bashrc
# rbenv
export PATH="$HOME/.rbenv/bin:$PATH"
eval "$(rbenv init -)"
# check
source ~/.bashrc
rbenv -v # show verson

#-----------------------------
# install ruby
#-----------------------------
rbenv install --list
rbenv install 2.5.1
rbenv global 2.5.1 # change to ruby 2.5.1
ruby -v # show version

#-----------------------------
# install bundle
#-----------------------------
gem install bundler
gem query -ra -n  "^rails$"

# SAMPLE: inatall rails
mkdir rails_v5.2.0 # お好きな名前を
cd rails_v5.2.0
bundle init # カレントディレクトリにGemfileを作成
echo 'gem "rails", "5.2.0"' >> Gemfile # ※ 任意のバージョンを指定
bundle install --path vendor/bundle # ./vendor/bundleにRails関連のGemが入る
```

# basic info

- top: https://www.ruby-lang.org/en/
- documentation: https://www.ruby-lang.org/en/documentation/
- Ruby Study Notes: http://rubylearning.com/satishtalim/tutorial.html
- Ruby Users Guide: http://www.rubyist.net/~slagell/ruby/
- Ruby Programming(sample code): https://en.wikibooks.org/wiki/Ruby_Programming

- japanese
  - Rubyトップ： https://www.ruby-lang.org/ja/
  - 文書一覧： https://www.ruby-lang.org/ja/documentation/
  - クイックスタート： https://www.ruby-lang.org/ja/documentation/quickstart/
  - 多言語からのRuby入門： https://www.ruby-lang.org/ja/documentation/ruby-from-other-languages/
  - Rubyリファレンス検索：https://docs.ruby-lang.org/ja/search/

### basic

- show version
```
ruby -v
```
- name rule

 - Abc   : 定数(大文字から始まる識別子は定数：定数は変更も可能だが警告が発生する。）
   - xx.rb:8: warning: already initialized constant XYZ
   - xx.rb:6: warning: previous definition of XYZ was here
 - $abc  : Global variable
 - @abc  : instance variable
 - @@abc : class variable

- true false 

  - Rubyでは、nilとfalseを除くすべてのものは真と評価されます。
  - 以下のコードを実行すると、Rubyでは"0 is true"が出力される。

```
# Ruby版
if 0
  puts "0 is true"
else
  puts "0 is false"
end
```

- interactive ruby

```
$ irb
irb(main):001:0> 1+2
=> 3
irb(main):002:0> Math.sqrt(9)
=> 3.0
```

- comment / doc
```
# this is comment
a=10   # comment

=begin
documents section
the everything between a line beginning with `=begin' and that with `=end' will be skipped by the interpreter.
=end
```
- 表示
```
# puts : print args with CR
puts 'abc';
puts( 'abc' );

# print : print args without CR
print 'abc';
print( 'abc' );

# p : 人間に分かりやすい形式で表示
p 100
p 'abc'
# output
# 100
# "abc"

# with valiable
puts "your name is #{ name }"

# with expression
a=10
puts "#{a}*#{a}+5 is #{a*a+5}"

# puts with ||
puts nil || 2008  
puts false || 2008  
puts "ruby" || 2008
# output
#  2008
#  2008
#  ruby

```

- command execute : コマンド実行

```
puts `dir`
puts `ls`
system( "ls -aF" )
```

- 変数: variable

```
a = 1
a_int = 1

s = 'hello'
s_string = 'hello'

# print 1
puts a 
puts a_int

# print hello
puts s
puts s_string

```

- 演算子
```
puts 1 + 1;
puts 1 - 1;
puts 1 * 1;
puts 4 / 2;

=begin  
 Ruby Numbers  
 Usual operators:  
 +     addition  
 -     subtraction  
 *     multiplication  
 /     division  
 ==    等しい
 !=    等しくない
 >     より大きい
 <     より小さい
 []    element参照
 []=   element set
 **    Exponentiation
 !     NOT
 ~     complement
 +     plus
 -     minus
 *     multiply
 /     divide
 %     module
 >> << shift
 &     "And"(bitwise)
 ^     Exclusive OR
 |     regular OR
 <= < > >=     Comparison
 <=> == === != =~ !=	Equality and pattern match operations
 &&    Logical AND
 ||    Logical OR
 ..    Range inclusive
 ...   Range Exclusive
 ?:    Ternary if-then-else
 = %= /= -= += |= &= >>= <<= *= &&= ||= **= ^=
       	       	     	Assignment（割り当て）
 not			Logical negation
 or and			Logical composition
 if unless while until	Expression modifiers
 begin/end 	 	Block expression    
=end  

```

- 文字列 : strings : charactor array

```
# basic
puts "Hello World"  
puts 'Hello World'  

# 文字の入力
input_val = gets;
puts input_val;

# 数字への変換
input_val = gets
num = input_val.to_i
puts num * 2

# 変換
val_int    = val.to_i
val_float  = val.to_f
val_string = val.to_s

# extract last CR : 末尾の改行コードを削除
name = gets.chomp


# String concatenation  
puts 'I like' + ' Ruby'  

# Escape sequence  
puts 'It\'s my Ruby'  

# New here, displays the string three times  
puts 'Hello' * 3  

# << appending to a string  
a = 'hello '  
a<<'world. 
I love this world...'  
puts a

# HEAR document
a = <<END_STR  
This is the string  
And a second line  
END_STR  
puts a
```

- 配列: array

  - format
    - ary = [ obj0, obj1, obj2, ... ]
    - table = [ [ obj0, obj1, obj2, ... ], [ obj0, obj1, obj2, ... ], ... ]

  - 配列数： ary.size

  - 最後に追加： ary.push <value>

  - 最後から削除： ret_val = ary.pop
    - ret_val : 削除された最後のオブジェクト

```
val = [ 123, 456, 789 ]
puts val[0]  	# 123
puts val[1]	# 456
puts val[2]	# 789
puts val.size	# 3
p val           # --> [123, 456, 789]

val.push 999
p val

v = []
v.push 1
v.push 2
v.push 3
p v

pop_val = val.pop
p val
p pop_val

tbl = [[0,1,2], [3,4,5], ['a','b','c']]
p tbl
p tbl[0]
p tbl[1]
p tbl[2]
p tbl[0][0]
p tbl[2][1]

# sample
tbl = [
  [1,1,1,0,0],
  [1,0,0,1,0],
]

y_max = tbl.size
y_max.times do |y|
  x_max = tbl[y].size
  x_max.times do |x|
    if tbl[y][x] == 1
      print "X"
    else
      print "-"
    end
  end
  print "\n"
end

```
- HASH : ハッシュ

```
hash_val = {
  'ABC' => 123,
  'DEF' => 456,
  'XYZ' => 789
}
p hash_val

h_val = {}
h_val['aaa'] = 10;
h_val['abc'] = 20;
h_val['z'] = 30;
p h_val

hash_val = {
  'ABC' => ['mesg0', 123],
  'DEF' => ['mesg1'],
  'XYZ' => ['mesg2', 1, 2, 3, 4],
}

```

- メソッド: method : ( function )

```
puts 'abc';
puts( 'abc' );

def print_name( name )
  puts name
end
print_name('foo')

def add(a, b, c)
  total = a + b + c
  return total
end
puts add(1, 2, 4)

def hi(name = "world")
  puts "Hello #{name}!"
end
hi()
hi("foo")
```

- class

  - instance variable
    - @<instance_variable>

  - show instance method
    - ClassName.instance_methods        # show all methods
    - ClassName.instance_methods(false) # defined only CalssName

  - check method
    - <instance>.respond_to?( "method_name" )

  - attribute accessor
    - attr_accessor :<atr_name>

  - access modifier
    - デフォルトでは、メソッドはpublic
    - privateアクセス修飾子はスコープの終わりか、他のアクセス修飾子があらわれるまで継続されます。 
    - public、private、protected
      - public : 誰でもアクセスできる
      - private: クラスのインスタンスからのみアクセスできる
      - protected: 以下からアクセスできる
      	- クラスおよび継承関係にあるクラスのインスタンス
	- クラスと同じパッケージにあるクラスのインスタンス

    ```
    class MyClass
      # a_methodはpublicです
      def a_method; true; end

      private

      # another_method, next_methodはprivateです
      def another_method; false; end
      def next_method; false; end
    end
    ```

  - クラスは開いている
    - いつでもクラスを開いて、定義を足したり、変更することができます。
    - Fixnumや、すべてのオブジェクトの祖先であるObjectのようなクラスであっても、自由に再定義することが可能

```
class Greeter
  def initialize(name = "World")
    # name : instance variable    
    @name = name
  end
  def say_hi
    puts "Hi #{@name}!"
  end
  def say_bye
    puts "Bye #{@name}, come back soon."
  end
end
# run class demo
greeter = Greeter.new("Pat")
greeter.say_hi
greeter.say_bye

# show all instance methods of Greeter
Greeter.instance_methods

# show instance methods defined into Greeter only (with out parent)
Greeter.instance_methods(false)

# add attr_accessor
class Greeter
  attr_accessor :name
end

greeter.name
=> "Pat"
greeter.say_hi
Hi Pat!

greeter.name = "Hiro"
=> "Hiro"
greeter.name
=> "Hiro"
greeter.say_hi
Hi Hiro!

```

- module

```
Math.sqrt()
```

- load

```
require "foo.rb"
```

- キーワード引数 : keyword arcument

  - メソッドはRuby 2.0から、Pythonのように、 キーワード引数を定義できるようになりました。

```
def deliver(from: "A", to: nil, via: "mail")
  "Sending from #{from} to #{to} via #{via}."
end

deliver(to: "B")
# => "Sending from A to B via mail."
deliver(via: "Pony Express", from: "B", to: "A")
# => "Sending from B to A via Pony Express."
```

### 制御

#### if
```
# if
a = 0
if a == 0
  puts 'TRUE : a is ZERO'
else
  puts 'FALSE : a is NOT ZERO'
end

if name.nil?
  puts 'name is nil !'
else
  puts 'name is NOT nil'
end

```
#### case

```
r = rand(3) + 1
case r
when 1
  puts 'r is 1'
when 2
  puts 'r is 2'
when 3
  puts 'r is 3'
end
```

#### loop / while

```
@names.each do |name|
  puts "Hello #{name}!"
end

# while
c = 0
while c < 5
  puts c
  c += 1
end
puts 'end of while loop'
puts c

# infinity loop
while true
  # infinity loop
end

# loop by times
loop_count = 10;
loop_count.times do
  puts 'hello'
end

# loop val
3.times do |n|
  puts n
end
# output
# 0
# 1
# 2

```

### 主なメソッド

```
sleep( 10 );  # sleep 10 sec

n=6;
rand( n ); # return 0-5 integer;

exit   # terminate program

name = gets.chomp # extract last CR

```
## tips

```
# メインファイルとして実行されている場合のチェック
#   __FILE__ は現在のファイル名を返す特別な変数
#   $0はプログラムを実行するときに使われるファイル名
if __FILE__ == $0

```

### JSON を YAML に変換する

```
$ ruby -ryaml -rjson -e 'puts YAML.dump(JSON.parse(STDIN.read))' < sample.json > sample.yaml

# sample
$ cat sample.json
{
  "Date":"2016-09-20",
  "Name":"classmethod, Inc"
}
$ sample.yaml
---
Date: '2016-09-20'
Name: classmethod, Inc

```

