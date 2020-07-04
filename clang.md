## options

```
-Weverything (clang only) : �������S�x����\��
-Wall
-Wextra
-pedantic/-Wpedantic

Turning On Specific Warnings
	-Wmissing-prototypes
	-Wformat-security
Turning Off Specific Warnings
	-Wno-deprecated-declarations
	-Wno-unused-parameter
```

- ref
  - https://gist.github.com/d0k/3608547
  - https://embeddedartistry.com/blog/2017/3/7/clang-weverything


## install

- https://stackoverflow.com/questions/44219158/how-to-install-clang-and-llvm-3-9-on-centos-7

```
$ sudo yum install centos-release-scl
$ yum install llvm-toolset-7
```

- Enable llvm-toolset-7:

```
$ scl enable llvm-toolset-7 bash
```

## ast

- man
```
clang -cc1 --help
```

- �V���{���ꗗ���擾
```
$ clang -cc1 -ast-list -fblocks -x objective-c Spam.h
```

- AST��dump
```
$ clang -cc1 -ast-dump -fblocks -x objective-c Spam.h
```

- AST��pretty-print
```
$ clang -cc1 -ast-print -fblocks -x objective-c Spam.h
```

- ����̃V���{���Ɋ֘A���镔���̂ݎ擾

  - ast-list�Ŏ擾�����V���{�������w�肵�āA���̃V���{���Ɋ֘A���镔���̂�dump����B

```
$ clang -cc1 -ast-dump -fblocks -x objective-c Spam.h -ast-dump-filter Spam
```

- Clang��AST���o�͂���ɂ�
  - https://www.hamayanhamayan.com/entry/2017/06/05/152925

```
clang -Xclang -ast-dump -fsyntax-only sample.c
```

## how to use

- clang��--analyze�I�v�V�����ŉ��
```
$ clang --analyze zzz.c
```

- scan-build�R�}���h��HTML���|�[�g���쐬
```
$ scan-build clang zzz.c
$ scan-view /tmp/scan-build-2015-04-26-145938-25383-1
```

- cmake�Ŏg�p����ꍇ
```
$ cmake -DCMAKE_C_COMPILER=/usr/libexec/clang-analyzer/scan-build/ccc-analyzer -DCMAKE_CXX_COMPILE=/usr/libexec/clang-analyzer/scan-build/c++-analyzer /path/to/src
$ make
```
