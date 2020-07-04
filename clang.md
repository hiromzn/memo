## options

```
-Weverything (clang only) : ＜＜＜全警告を表示
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

- シンボル一覧を取得
```
$ clang -cc1 -ast-list -fblocks -x objective-c Spam.h
```

- ASTをdump
```
$ clang -cc1 -ast-dump -fblocks -x objective-c Spam.h
```

- ASTをpretty-print
```
$ clang -cc1 -ast-print -fblocks -x objective-c Spam.h
```

- 特定のシンボルに関連する部分のみ取得

  - ast-listで取得したシンボル名を指定して、そのシンボルに関連する部分のみdumpする。

```
$ clang -cc1 -ast-dump -fblocks -x objective-c Spam.h -ast-dump-filter Spam
```

- ClangでASTを出力するには
  - https://www.hamayanhamayan.com/entry/2017/06/05/152925

```
clang -Xclang -ast-dump -fsyntax-only sample.c
```

## how to use

- clangの--analyzeオプションで解析
```
$ clang --analyze zzz.c
```

- scan-buildコマンドでHTMLレポートを作成
```
$ scan-build clang zzz.c
$ scan-view /tmp/scan-build-2015-04-26-145938-25383-1
```

- cmakeで使用する場合
```
$ cmake -DCMAKE_C_COMPILER=/usr/libexec/clang-analyzer/scan-build/ccc-analyzer -DCMAKE_CXX_COMPILE=/usr/libexec/clang-analyzer/scan-build/c++-analyzer /path/to/src
$ make
```
