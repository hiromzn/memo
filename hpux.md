- list the content:
```
swlist -s /full/path_to_file.depot
swlist -l subproduct -s /absolute_path/xxx.depot
swlist -l fileset -s /absolute_path/xxx.depot
swlist -l file -s /absolute_path/xxx.depot
```

- install:
```
# launch character window
swinstall -s /full/path_to_file.depot

# install all
swinstall -s /full/path_to_file.depot \*

# install log
vi /var/adm/sw/swagent.log
```
