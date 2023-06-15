# less
- -R / -r
  - row mode print

# sed
- merge line with next line of specific pattern
```
$ cat input.txt
Line 1
Pattern
Line 2
Pattern
Line 3

$ sed -n '/Pattern/{N;s/\n/ /;p}' input.txt

Pattern Line 2
Pattern Line 3
```
