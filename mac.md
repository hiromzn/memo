## short cut

| key | description |
|-----+-----|
paste without format | Option + Cmd + Shift + V


## backup

### tmutil

- timemachine command line tool

```
tmutil snapshot
tmutil listbackups

$ cd /Volumes/MobileBackups/Backups.backupdb/MyMini
$ tmutil compare 2012-04-20-160854 2012-04-20-160951
```

## doxygen

```
cd project/src
doxygen -g
vi Doxyfile
doxygen
open html/index.html
```

- sample of Doxyfile
```
$ diff -y --suppress-common-lines -W80 Doxyfile Doxyfile.default 
EXTRACT_ALL            = YES	      |	EXTRACT_ALL            = NO
RECURSIVE              = YES	      |	RECURSIVE              = NO
SOURCE_BROWSER         = YES	      |	SOURCE_BROWSER         = NO
INLINE_SOURCES         = YES 	      |	INLINE_SOURCES         = NO
GENERATE_LATEX         = NO 	      |	GENERATE_LATEX         = YES
HAVE_DOT               = YES	      |	HAVE_DOT               = NO
UML_LOOK               = YES	      |	UML_LOOK               = NO
CALL_GRAPH             = YES	      |	CALL_GRAPH             = NO
CALLER_GRAPH           = YES	      |	CALLER_GRAPH           = NO
```

## graphviz

#### source
- git clone https://gitlab.com/graphviz/graphviz.git

#### prepare

- brew install automake autogen libtool
  - when ./autogen.sh got the following error
    - can not find auto
    - error: possibly undefined macro: AC_ENABLE_STATIC 
    - If this token and others are legitimate, 
    - please use m4_pattern_allow. 
    - See the Autoconf documentation.

#### make
```
cd graphviz
./autogen.sh
./configure
make
make install
```

## backslash

- problem : can NOT input '\' key on terminal window.

- FIX:
  - change configiguration

    ```
    system_env_config
    -> keyboard
    -> input source
    -> input char for '\' key
    ```

## brew

- install tool

- basic usage

```
# update brew
brew update

brew search git
brew install cowsay
```

- install master

  ``` /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)" ```

  - ERROR

    ```
    The requested URL returned error: 404 Not Found
    Error: Failed to download resource "readline--patch"
    Download failed: https://gist.githubu....
    ```

- config
```
#
# access token for Homebrew
#
# 
# Error: GitHub API Error: API rate limit exceeded for 153.229.132.83. 
# (But here's the good news: Authenticated requests get a higher rate limit. 
# Check out the documentation for more details.)
# Try again in 59 minutes 41 seconds, or create a personal access token:
#   https://github.com/settings/tokens/new?scopes=&description=Homebrew
# and then set the token as: export HOMEBREW_GITHUB_API_TOKEN="your_new_token"
```

## gcc
```
$ brew install gcc
```

## cmake

- https://cmake.org/
- https://cmake.org/download/

```
$ tar zxvf cmake-3.8.2.tar.gz
$ rm zxvf cmake-3.8.2.tar.gz

$ cd cmake-3.8.2
$ ./bootstrap
$ make
$ make install

$ which cmake
/usr/local/bin/cmake
```

## llvm / clang

#### install

```
$ brew install --with-toolchain llvm
.......
LLVM executables are installed in /usr/local/opt/llvm/bin.
Extra tools are installed in /usr/local/opt/llvm/share/llvm.

This formula is keg-only, which means it was not symlinked into /usr/local.

OS X already provides this software and installing another version in parallel can cause all kinds of trouble.

Generally there are no consequences of this for you. If you build your own software and it requires this formula, you'll need to add to your build variables:

    LDFLAGS:  -L/usr/local/opt/llvm/lib
    CPPFLAGS: -I/usr/local/opt/llvm/include

If you need Python to find bindings for this keg-only formula, run:
  echo /usr/local/opt/llvm/lib/python2.7/site-packages >> /usr/local/lib/python2.7/site-packages/llvm.pth
  mkdir -p /Users/hmizuno/Library/Python/2.7/lib/python/site-packages
  echo 'import site; site.addsitedir("/usr/local/lib/python2.7/site-packages")' >> /Users/hmizuno/Library/Python/2.7/lib/python/site-packages/homebrew.pth
==> Summary
/usr/local/Cellar/llvm/3.6.2: 813 files, 229.6M, built in 21 minutes 37 seconds
```

```
# before brew
$ clang -v
Apple LLVM version 9.1.0 (clang-902.0.39.2)
Target: x86_64-apple-darwin17.6.0
Thread model: posix
InstalledDir: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin

# after brew

```
######################################
port (MacPort) : update tool of Mac !
######################################

port search <name>
port deps <portname> ..... dependency 依存関係調査
port contents <portname> ..... file lists of portname
port installed ...... portname list which is installed on your mac.
port outdated ....... updateの確認

sudo port install <portname> ..... install

sudo port selfupdate ............. port command自体のアップデート
     	  	     		   一回実行すれば後はoutdatedで管理する。

## Java

- OpenJDK

  - install

    ```
    brew tap adoptopenjdk/openjdk
    brew install --cask adoptopenjdk8
    brew install --cask adoptopenjdk9
    brew install --cask adoptopenjdk10
    brew install --cask adoptopenjdk11
    ```

######
java
######
download
	http://www.oracle.com/technetwork/java/javase/downloads/

インストールされているJDKのバージョン一覧表示

	% /usr/libexec/java_home -V
	Matching Java Virtual Machines (2):
	    1.8.0, x86_64:      "Java SE 8"     /Library/Java/JavaVirtualMachines/jdk1.8.0.jdk/Contents/Home
	    1.7.0_45, x86_64:   "Java SE 7"     /Library/Java/JavaVirtualMachines/jdk1.7.0_45.jdk/Contents/Home

Javaのバージョン毎のHOMEを確認
	$ /usr/libexec/java_home -v 1.7
		/Library/Java/JavaVirtualMachines/jdk1.7.0_51.jdk/Contents/Home
	$ /usr/libexec/java_home -v 1.6
		/System/Library/Java/JavaVirtualMachines/1.6.0.jdk/Contents/Home

softwareupdate ..... 
	installer .... install *.pkg files


現在有効なJDKのバージョンを出す

	% /usr/libexec/java_home
	/Library/Java/JavaVirtualMachines/jdk1.8.0.jdk/Contents/Home

JDKのバージョンを切り替える

	% export JAVA_HOME=`/usr/libexec/java_home -v 1.7.0_45`

	% java -version
	java version "1.7.0_45"

	% export JAVA_HOME=`/usr/libexec/java_home -v 1.8.0`
	% java -version
	java version "1.8.0"
