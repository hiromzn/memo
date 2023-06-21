### tips
```
# silent option
.SILENT :
```

### sample

```
$(addprefix -I,$(VAR))
$(addsuffix .bak,$(VAR))
$(dir $(files))			# ./foo/bar.c --> ./foo
$(notdir $(files))		# ./foo/bar.c --> bar.c
$(basename $(files))	# delete suffix ./foo/bar.c --> ./foo/bar
```

```
#
# Hello Worldを出力する
#
all:
	@echo Hello World!

object_files = \
   foo.o \
   bar.o \
   baz.o

ignore_ERROR :
	- mkdir ./exist_dir/

```

```
#
# create EXE file : foo.c ==> foo
#
srcs = $(wildcard *.c)

tgt = $(srcs:.c=)

all : $(tgt)

$(tgt): %: %.c
        $(CC) $(CFLAGS) $< -o $@

clean :
        rm -f $(tgt)
```

```
#
# create OBJ file : foo.c ==> foo.o
#
srcs = $(wildcard *.c)

objs = $(srcs:.c=)

all : $(objs)

$(objs): %.o: %.c
        $(CC) -c $(CFLAGS) $< -o $@

clean :
        rm -f $(objs)
```

```
# run command and get stdout
UNAME = $(shell whoami)
```

## macro

```
.c.o:
	cc -c -o $@ $<
```

- $< : ソースファイル名(foo.c)
- $@ : ターゲット名（foo.o）
- $* : 拡張子を除いたターゲット名（foo）
- $% : ターゲットがアーカイブメンバだったときのターゲットメンバ名
- $? : ターゲットより新しいすべての依存するファイル名

## sample

- get source file list from command dynamically.

```
all:
        @for target in $(TARGETS); do \
        ( \
                cd $$target; \
                make; \
        ) \
        done
```

```
SRCDIR = ./src
GETSRCS = `echo $(SRCDIR)/ca*.c`

OBJS = $(SRCS:.c=.o)

# CodeAdvisor
PDB = ./pdb

all :
        make SRCS="$(GETSRCS)" objs

objs : $(OBJS)

.c.o :
        $(CC) $(COPT) -c $<
```

```
# check using touch file
target : $(target_files) $(CONVERT_FILES)

$(CONVERT_FILES) :
	make convert_files

convert_files :
	./convert_files.sh $(TARGET_DIR)
	touch $(CONVERT_FILES)
```
