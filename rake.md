## ��{����

#### install
```
gem install rake
```

#### run & operations
```
rake -f <file_name>
rake <task_name>			# �w��^�X�N�����s
rake <pram_key>=<value> <task_name>	# �p�����[�^���w�肵�ă^�X�N�����s
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
desc "�^�X�N�̐������L�q"
task <task_name>, [<parm>, <parm, ...] => [<���Otask>, <���Otask>, ...] do
  <action>
  puts args.<parm>	# <parm>�̎Q��
end
```

- desc : ���ɒ�`����^�X�N�̐������L�q����B�^�X�N�ꗗ�\��(-T)���ɐ��������\�������B
- task_name : �^�X�N�����ʂ��閼�O
- parm : �^�X�N�ɓn�����p�����[�^��
- ���Otask : �^�X�N�����s�����܂łɎ��s�����^�X�N�̈ꗗ�B���s����Ă��Ȃ���Ύ��s�����B
- action : �^�X�N�Ŏ��s����鏈��

#### file, directory
```
file "file_name" do
  <action>
end

directory "dir_name"
```
- file : <file_name>�����݂��Ȃ��ꍇ�Ɏ��s����^�X�N�̒�`
- directory : dir_name�����݂��Ȃ��ꍇ�ɁA�f�B���N�g�����쐬����^�X�N�iaction�͎w��ł��Ȃ��j

#### rule
```
rule /.*\.txt/ do |t|
  puts "create #{t.name}"
  open("#{t.name}", "w"){|f| f << "test." }
end

```
- rule
  - �^�X�N���������t�@�C�������݂��Ȃ��ꍇ�Ɏ��s����^�X�N���`
    - ���̏ꍇ�F�@*.txt �Ƀ}�b�`����foo.txt�Ƃ����^�X�N�����w�肳��Afoo.txt�����݂��Ȃ��ꍇ�Ƀ��b�Z�[�W��\�����ăt�@�C�����쐬����^�X�N�̒�`

#### namespace
```
namespace :<name_space_name> do
  task :hello do
    puts "hello!"
  end
end
```
- task��`�Ȃǂ̖��O��Ԃ��`����B
- task�̎w��́A<name_space_name>:hello �Ŏw�肷��B

#### Rakefile pattern

- nested task

  - 2�̃^�X�N���`
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

