## 基本操作

#### install
```
gem install rake
```

#### run & operations
```
rake -f <file_name>
rake <task_name>			# 指定タスクを実行
rake <pram_key>=<value> <task_name>	# パラメータを指定してタスクを実行
rake -h			# prnit help
rake -T      		# show task list
```
- default file_name: ./Rakefile


#### hello world

```
task default: :hello

desc "hello"
task :hello do
  puts "Hello world!!"
end
```

#### task
```
# format
desc "タスクの説明を記述"
task <task_name>, [<parm>, <parm, ...] => [<事前task>, <事前task>, ...] do
  <action>
  puts args.<parm>	# <parm>の参照
end
```

- desc : 次に定義するタスクの説明を記述する。タスク一覧表示(-T)時に説明文が表示される。
- task_name : タスクを識別する名前
- parm : タスクに渡されるパラメータ名
- 事前task : タスクが実行されるまでに実行されるタスクの一覧。実行されていなければ実行される。
- action : タスクで実行される処理

#### file, directory
```
file "file_name" do
  <action>
end

directory "dir_name"
```
- file : <file_name>が存在しない場合に実行するタスクの定義
- directory : dir_nameが存在しない場合に、ディレクトリを作成するタスク（actionは指定できない）

#### rule
```
rule /.*\.txt/ do |t|
  puts "create #{t.name}"
  open("#{t.name}", "w"){|f| f << "test." }
end

```
- rule
  - タスク名が示すファイルが存在しない場合に実行するタスクを定義
    - 上例の場合：　*.txt にマッチするfoo.txtというタスク名が指定され、foo.txtが存在しない場合にメッセージを表示してファイルを作成するタスクの定義

#### namespace
```
namespace :<name_space_name> do
  task :hello do
    puts "hello!"
  end
end
```
- task定義などの名前空間を定義する。
- taskの指定は、<name_space_name>:hello で指定する。

#### Rakefile pattern

- nested task

  - 2つのタスクを定義
    - basic:hello
    - foo:bar:hoge

  - how to run
    - rake -f <fname> basic:hello
    - rake -f <fname> foo:bar:hoge
    
```
task default: 'basic:hello'

namespace :basic do
  desc "hello"
  task :hello do
    puts "Hello Rake!!"
  end
end

namespace :foo do
  namespace :bar do
    desc "foo bar hoge!!"
    task :hoge do
      puts "foo:bar:hoge!!"
    end
  end
end
```

- task chain

```
desc "invoke foo bar"
task :hello => %w[foo bar]

task :foo do
  puts "hello foo"
end

task :bar do
  puts "hello bar"
end
```

- pass argument

```
desc "hello task"
task :hello, [:name, :job] => [:hi, :bye]


task :hi, [:name, :job] do |task, args|
  puts "DEBUG: #{task}, #{args}"
  puts "Hi #{args[:name]}, #{args[:job]}"
end

task :bye, [:name, :job] do |task, args|
  puts "DEBUG: #{task}, #{args}"
  puts "bye #{args[:name]}, #{args[:job]}"
end
```

