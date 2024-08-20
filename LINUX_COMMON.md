# Linux Cheat Sheet

## Output Redirect
### Redirect both stdout and error to a file
```
$ unittest.sh > tests.log 2>&1
```

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
-i
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
ln -s /mnt/target /home/david/link
```
### Display or manipulate disk partition table
(Find disk)
Useful options
- -l - display partitions
```
fdisk -l
```

## System
### Get Kernel Release
```
uname -r
```
### Get Architecture
```
uname -m
```
### Get CPU info
```
lscpu
```
### XAuthority for remote desktop
[.Xauthority file creation](https://superuser.com/questions/806637/xauth-not-creating-xauthority-file)
```
# Rename the existing .Xauthority file by running the following command
mv .Xauthority old.Xauthority 

# xauth with complain unless ~/.Xauthority exists
touch ~/.Xauthority

# only this one key is needed for X11 over SSH 
xauth generate :0 . trusted 

# generate our own key, xauth requires 128 bit hex encoding
xauth add ${HOST}:0 . $(xxd -l 16 -p /dev/urandom)

# To view a listing of the .Xauthority file, enter the following 
xauth list
```
To Verify X11Forwarding parameter:
```
sudo cat /etc/ssh/sshd_config |grep -i X11Forwarding
```
You should see similar output as the following:
```
X11Forwarding yes
```
### Alternatives management
[Baeldung manual](https://www.baeldung.com/linux/update-alternatives-command)
#### List alternatives
```
update-alternatives --get-selections
```
#### Create alternative for ```jetbrains```
```
sudo update-alternatives --install /usr/bin/jetbrains-toolbox-2.4.2.32922/jetbrains-toolbox jetbrains /usr/bin/jetbrains 100
```
#### Display alternative for ```jetbrains```
```
update-alternatives --display jetbrains
```
#### Delete alternative for ```jetbrains```
```
sudo update-alternatives --remove jetbrains /usr/bin/jetbrains
```
