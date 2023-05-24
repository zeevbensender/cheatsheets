# Linux Cheat Sheet

### Add Line To File
```
$ cat >> <file name>
write here content
to be added, then hit Ctrl+C
```

### The ```less``` command
#### Search ignore case
```
$ less
-n
/<pattern>
```

### Create soft link
```
$ ln -s file1 link1
```

### Validating File CHECKSUM
```
$ sha256sum <file name>
```
