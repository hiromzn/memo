## command line

- $ gdb executable

## basic
```
# bp
b <line_num>		... set break pointer
i b							... show break pointer
f <n>						... change frame <n>
up							... change up frame
down						... change down frame

i locals				... show local variables of this frame
i args					... show arguments of this frame

info stack
info thread

p <exp>					... print <exp>
x/<n> <exp>			... print hex dump <exp>

```

## INFO
```
bt ................ print stack tarce
list funcname ..... 関数の内容の表示
p name ............ 変数の内容の表示
info threads ...... threadの一覧表示
info locals ....... local変数の表示
info args ......... argumentの表示
```

## print / dump

- print data

```
p/<type> <exp>

sample:
  p mydata
  p/x buffer

# print as a structure
pt <exp>

# Display structure members line by line
set print pretty on
# Display structure members in one line
set print pretty off
```

- dump memory

```
x/<n><type><size> <address>

<n> ...... number of data
<type> ... data show type
<size> ... size of data (b(1byte), h(2byte), w(4byte), g(8byte))

<type>:
x .... unsigned hex
d .... signed decimal
u .... unsigned decimal
o .... unsigned octal
a .... address
c .... character
f .... float
s .... string which terminated by null
i .... assemble
```

## THREAD

- info threads ...... threadの一覧表示
- thread # .......... 指定したthreadにスイッチする

## BREAKPOINTS
```
b main ............ set bp at function
b line_number ..... set bp at line number in current source
i b ............... print break pointer information
d <n> ............. delete <n> bp
dis <n> ........... disable <n> bp
ena <n> ........... enable <n> bp
commands .......... add command into last BreakPoint
	(gdb) commands
	Type commands for when breakpoint 9 is hit, one per line.
	End with a line saying just "end".
	>p arg1
	>end
commands <n> ...... <n>番目のbpにcommandを登録
```

### RUN
```
run ............... 実行開始
  run argments
  run -a -b file <infile

c ................. 次のBPまで実行
n ................. next プログラムを1行実行
ni ................ 次のmachine instructionを実行
s ................. ステップ実行
fin ............... (finish)現在選択しているstack frameの
                    関数を最後まで実行する

att <PID> ......... PIDで指定したプロセスにattach
                    する。
det ............... プロセスをデタッチする
```

### ENVIEONMENT
```
frame n ........... stack frameの変更
dir <dirname> ..... soruceが入っているdirの指定
show dir .......... soruceが入っているdirの設定値の表示
objectdir <dirname> ... objectが入っているdirの指定
```
## ASSEMBLE DEBUG
```
> disp/10i $pc .... auto print
> si .............. step instruction
> ni .............. next instruction
> i r ............. print registers

> i disp
Auto-display expressions now in effect:
Num Enb Expression
2:   y  /10bi $ip
> del display 2
```

### init file
```
 $ cat $HOME/.gdbinit
 add-auto-load-safe-path /tmp/foo/.gdbinit

 $ pwd
 /tmp/foo
 $ cat ./.gdbinit
 b main
 $ gdb myexecute
```

## tools

#### memory leak tools of HP GDB
```
  from:
    http://h21007.www2.hp.com/dspp/tech/tech_TechDocumentDetailPage_IDX/1,1701,2078,00.html

  HP WDB supports batch mode memory leak
  detection, also called batch Run Time Checking
  (RTC). You can specify the batch mode
  configuration through a configuration file
  called rtcconfig. The configuration file
  supports these variables: 

    check_heap=on|off
	To enable leak detection 

    check_leaks=on|off
	To enable heap info 

    scramble_blocks=on|off
	To enable block scrambling 

    min_leak_size=block_size
	Minimum block size to use for leak detection 

    frame_count=no_frames
	Number of frames to be printed for leak context 

    output_dir=output_data_dir
	Name of the output data directory 

    files=file1:file2:...|fileN>
	Executables for which memory leak detection is enabled. 

  Batch mode leak detection stops the application
  at the end, when libraries are being unloaded,
  and invokes HP WDB to print the leak or heap
  data. 


  To use batch memory leak detection:

  Step 1. Create an rtcconfig file in the current
    directory. 

  Step 2. Define an environment variable
    BATCH_RTC. If you are using the Korn or Posix
    shell, type:

    export BATCH_RTC=on  

  Step 3. Start the target application by defining
    LD_PRELOAD of librtc. For example,

    LD_PRELOAD=/opt/langtools/lib/hpux32/librtc.sl application_name
  LD_PRELOAD=/opt/langtools/lib/hpux64/librtc.sl application_name 

  Step 4. At the end of the run output data file
    is created in the output_data_dir, if defined
    in rtcconfig, or the current directory. 

  HP WDB creates output data file for each run. It
  creates a separate file for leak detection and
  heap information. 


  The naming convention for output files is:
  file_name.pid.suffix 

  The value for suffix could be either leaks or heap. 
 


  Batch memory leak detection uses these
  environment variables:

  GDBRTC_CONFIG
    Specifies the location of rtcconfig. 

  BATCH_RTC
    Enables or disable batch memory leak detection. 

  GDB_SERVER
    By default, memory leak detection uses
    /opt/langtools/bin/gdb to print
    output. Override this with GDB_SERVER. 
```
