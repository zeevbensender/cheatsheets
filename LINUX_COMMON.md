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

## Storage
### List block devices
```
$ lsblk
```
### List SCSI block devices
```
$ lsblk -S
```
### Detect disk format
```
$ disktype /dev/sdc1
```
### Mount a device to the mountpoint
[Tutorial on youtube](https://www.youtube.com/watch?v=F-a_BBAGfkE)
Useful options:
- -t - set filesystem type
- --bind - Remount part of the file hierarchy
```
$ mount /dev/sdc1 /mnt/mydisk
```
### Unmount a device
```
$ umount /dev/sdc1
```
or
```
$ umount /mnt/mydisk
```
### Disk space usage
Useful options: 
- -h - human readable
- -T - show file system type
```
$ df -hT
```
### Estimate file space usage
Useful options: 
- -h - human readable
- -c - total
- -s - summarize
```
$ du -hcs /home/david/work/*
```
### Make link between files
Useful options
- -s - symbolic link
```
$ ln -s /mnt/target /home/david/link
```