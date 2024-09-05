# Linux Cheat Sheet

## Output Redirect
### Redirect both stdout and error to a file
```bash
unittest.sh > tests.log 2>&1
```

### Add Line To File
```bash
cat >> <file name>
write here content
to be added, then hit Ctrl+C
```

### The ```less``` command
#### Search ignore case
```bash
less
-i
/<pattern>
```


### Create soft link
```bash
ln -s file1 link1
```

### Validating File CHECKSUM
```bash
sha256sum <file name>
```

## Storage
### List block devices
```bash
lsblk
```
### List SCSI block devices
```bash
lsblk -S
```
### Detect disk format
```bash
disktype /dev/sdc1
```
### Mount a device to the mountpoint
[Tutorial on youtube](https://www.youtube.com/watch?v=F-a_BBAGfkE)
Useful options:
- -t - set filesystem type
- --bind - Remount part of the file hierarchy
```bash
mount /dev/sdc1 /mnt/mydisk
```
### Unmount a device
```bash
umount /dev/sdc1
```
or
```bash
umount /mnt/mydisk
```
### Disk space usage
Useful options: 
- -h - human readable
- -T - show file system type
```bash
df -hT
```
### Estimate file space usage
Useful options: 
- -h - human readable
- -c - total
- -s - summarize
```bash
du -hcs /home/david/work/*
```
### Make link between files
Useful options
- -s - symbolic link
```bash
ln -s /mnt/target /home/david/link
```
### Display or manipulate disk partition table
(Find disk)
Useful options
- -l - display partitions
```bash
fdisk -l
```

## System
### Get Kernel Release
```bash
uname -r
```

### Get Architecture

```bash
uname -m
```
### Get CPU info
```bash
lscpu
```
### XAuthority for remote desktop
[.Xauthority file creation](https://superuser.com/questions/806637/xauth-not-creating-xauthority-file)
```bash
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
```bash
sudo cat /etc/ssh/sshd_config |grep -i X11Forwarding
```
You should see similar output as the following:
```bash
X11Forwarding yes
```
### Alternatives management
[Baeldung manual](https://www.baeldung.com/linux/update-alternatives-command)
#### List alternatives
```bash
update-alternatives --get-selections
```
#### Create alternative for ```jetbrains```
```bash
sudo update-alternatives --install /usr/bin/jetbrains-toolbox-2.4.2.32922/jetbrains-toolbox jetbrains /usr/bin/jetbrains 100
```
#### Display alternative for ```jetbrains```
```
update-alternatives --display jetbrains
```bash
#### Delete alternative for ```jetbrains```
```bash
sudo update-alternatives --remove jetbrains /usr/bin/jetbrains
```

## Bash Tricks
### awk
#### Print local dir in ```awk```
```bash
# run the 'run.sh' script from subdirs having 'scripts' in their name
ls | grep scripts | awk ' { print "cd " ENVIRON["PWD"] "/" $1 " && ../run.sh"} '
```
