# tools

- gem

```
$ gem help
RubyGems is a sophisticated package manager for Ruby.
```

- rake

```
Rake�Ƃ� Ruby�ŋL�q���ꂽ�r���h�c�[��

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
mkdir rails_v5.2.0 # ���D���Ȗ��O��
cd rails_v5.2.0
bundle init # �J�����g�f�B���N�g����Gemfile���쐬
echo 'gem "rails", "5.2.0"' >> Gemfile # �� �C�ӂ̃o�[�W�������w��
bundle install --path vendor/bundle # ./vendor/bundle��Rails�֘A��Gem������
```

# basic info

- top: https://www.ruby-lang.org/en/
- documentation: https://www.ruby-lang.org/en/documentation/
- Ruby Study Notes: http://rubylearning.com/satishtalim/tutorial.html
- Ruby Users Guide: http://www.rubyist.net/~slagell/ruby/
- Ruby Programming(sample code): https://en.wikibooks.org/wiki/Ruby_Programming

- japanese
  - Ruby�g�b�v�F https://www.ruby-lang.org/ja/
  - �����ꗗ�F https://www.ruby-lang.org/ja/documentation/
  - �N�C�b�N�X�^�[�g�F https://www.ruby-lang.org/ja/documentation/quickstart/
  - �����ꂩ���Ruby����F https://www.ruby-lang.org/ja/documentation/ruby-from-other-languages/
  - Ruby���t�@�����X�����Fhttps://docs.ruby-lang.org/ja/search/

### basic

- show version
```
ruby -v
```
- name rule

 - Abc   : �萔(�啶������n�܂鎯�ʎq�͒萔�F�萔�͕ύX���\�����x������������B�j
   - xx.rb:8: warning: already initialized constant XYZ
   - xx.rb:6: warning: previous definition of XYZ was here
 - $abc  : Global variable
 - @abc  : instance variable
 - @@abc : class variable

- true false 

  - Ruby�ł́Anil��false���������ׂĂ̂��̂͐^�ƕ]������܂��B
  - �ȉ��̃R�[�h�����s����ƁARuby�ł�"0 is true"���o�͂����B

```
# Ruby��
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
- �\��
```
# puts : print args with CR
puts 'abc';
puts( 'abc' );

# print : print args without CR
print 'abc';
print( 'abc' );

# p : �l�Ԃɕ�����₷���`���ŕ\��
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

- command execute : �R�}���h���s

```
puts `dir`
puts `ls`
system( "ls -aF" )
```

- �ϐ�: variable

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

- ���Z�q
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
 ==    ������
 !=    �������Ȃ�
 >     ���傫��
 <     ��菬����
 []    element�Q��
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
       	       	     	Assignment�i���蓖�āj
 not			Logical negation
 or and			Logical composition
 if unless while until	Expression modifiers
 begin/end 	 	Block expression    
=end  

```

- ������ : strings : charactor array

```
# basic
puts "Hello World"  
puts 'Hello World'  

# �����̓���
input_val = gets;
puts input_val;

# �����ւ̕ϊ�
input_val = gets
num = input_val.to_i
puts num * 2

# �ϊ�
val_int    = val.to_i
val_float  = val.to_f
val_string = val.to_s

# extract last CR : �����̉��s�R�[�h���폜
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

- �z��: array

  - format
    - ary = [ obj0, obj1, obj2, ... ]
    - table = [ [ obj0, obj1, obj2, ... ], [ obj0, obj1, obj2, ... ], ... ]

  - �z�񐔁F ary.size

  - �Ō�ɒǉ��F ary.push <value>

  - �Ōォ��폜�F ret_val = ary.pop
    - ret_val : �폜���ꂽ�Ō�̃I�u�W�F�N�g

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
- HASH : �n�b�V��

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

- ���\�b�h: method : ( function )

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
    - �f�t�H���g�ł́A���\�b�h��public
    - private�A�N�Z�X�C���q�̓X�R�[�v�̏I��肩�A���̃A�N�Z�X�C���q���������܂Ōp������܂��B 
    - public�Aprivate�Aprotected
      - public : �N�ł��A�N�Z�X�ł���
      - private: �N���X�̃C���X�^���X����̂݃A�N�Z�X�ł���
      - protected: �ȉ�����A�N�Z�X�ł���
      	- �N���X����ьp���֌W�ɂ���N���X�̃C���X�^���X
	- �N���X�Ɠ����p�b�P�[�W�ɂ���N���X�̃C���X�^���X

    ```
    class MyClass
      # a_method��public�ł�
      def a_method; true; end

      private

      # another_method, next_method��private�ł�
      def another_method; false; end
      def next_method; false; end
    end
    ```

  - �N���X�͊J���Ă���
    - ���ł��N���X���J���āA��`�𑫂�����A�ύX���邱�Ƃ��ł��܂��B
    - Fixnum��A���ׂẴI�u�W�F�N�g�̑c��ł���Object�̂悤�ȃN���X�ł����Ă��A���R�ɍĒ�`���邱�Ƃ��\

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

- �L�[���[�h���� : keyword arcument

  - ���\�b�h��Ruby 2.0����APython�̂悤�ɁA �L�[���[�h�������`�ł���悤�ɂȂ�܂����B

```
def deliver(from: "A", to: nil, via: "mail")
  "Sending from #{from} to #{to} via #{via}."
end

deliver(to: "B")
# => "Sending from A to B via mail."
deliver(via: "Pony Express", from: "B", to: "A")
# => "Sending from B to A via Pony Express."
```

### ����

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

### ��ȃ��\�b�h

```
sleep( 10 );  # sleep 10 sec

n=6;
rand( n ); # return 0-5 integer;

exit   # terminate program

name = gets.chomp # extract last CR

```
## tips

```
# ���C���t�@�C���Ƃ��Ď��s����Ă���ꍇ�̃`�F�b�N
#   __FILE__ �͌��݂̃t�@�C������Ԃ����ʂȕϐ�
#   $0�̓v���O���������s����Ƃ��Ɏg����t�@�C����
if __FILE__ == $0

```

### JSON �� YAML �ɕϊ�����

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

