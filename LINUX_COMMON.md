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

Display Partition types
```bash
df -T
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
### Get Linux Distribution
```bash
uname -a
```


### Get CPU info
```bash
lscpu
```
### XAuthority for remote desktop
[.Xauthority file creation](https://superuser.com/questions/806637/xauth-not-creating-xauthority-file)
```bash
# Ensure your Ubuntu machine has X11-related packages installed:
sudo apt update
sudo apt install -y openssh-server xauth x11-apps

# Rename the existing .Xauthority file by running the following command
mv .Xauthority old.Xauthority 

# xauth with complain unless ~/.Xauthority exists
touch ~/.Xauthority

# only this one key is needed for X11 over SSH 
xauth generate :0 . trusted 

# Verify that the DISPLAY variable is set by running
echo $DISPLAY

# If it's empty, manually set it
export DISPLAY=localhost:10.0

# generate our own key, xauth requires 128 bit hex encoding
xauth add ${HOST}:0 . $(xxd -l 16 -p /dev/urandom)

# To view a listing of the .Xauthority file, enter the following 
xauth list
```
To Verify X11Forwarding parameter:
```bash
sudo cat /etc/ssh/sshd_config |grep -i X11
```
You should see similar output as the following:
```bash
X11Forwarding yes
X11DisplayOffset 10
X11UseLocalhost yes
```
#### To reset existing XAuthority config
```bash
# Clean Up X11 Authorization
xauth remove $DISPLAY
xauth remove localhost:10.0
xauth remove :10

# Reset the Xauthority file
rm -f ~/.Xauthority
touch ~/.Xauthority
chmod 600 ~/.Xauthority

# Verify that xauth list shows nothing
xauth list

# Restart SSH to clear any old sessions
sudo systemctl restart sshd

# Exit and reconnect
exit
ssh -X zeevb@your_remote_host

Verify the DISPLAY variable:
# echo $DISPLAY
# Expected output: localhost:10.0

# Verify that current magic cookie is printed
xauth list
#Expected output: YOUR_HOST_NAME/unix:10  MIT-MAGIC-COOKIE-1  COOKIE_VALUE
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

## User Management

### Add User
```bash
adduser zeevb
# Below is the expected output:
Adding user `zeevb' ...
Adding new group `zeevb' (1001) ...
Adding new user `zeevb' (1001) with group `zeevb' ...
Creating home directory `/home/zeevb' ...
Copying files from `/etc/skel' ...
New password:
Retype new password:
passwd: password updated successfully
Changing the user information for zeevb
Enter the new value, or press ENTER for the default
        Full Name []: Lupo
        Room Number []:
        Work Phone []:
        Home Phone []:
        Other []:
```
### Grant the user sudo privileges
```bash
usermod -aG sudo zeevb
# verify the user was added to the sudo group
groups zeevb
# expected output -> zeevb : zeevb sudo
```
### Avoid identification requirement upon running sudo commands
```
vi /etc/sudoers
# Modify user privilege specification. Add the line for zeevb after the root:
# root    ALL=(ALL:ALL) ALL
# zeevb   ALL=(ALL:ALL) ALL
```

## Bash Tricks
### awk
#### Print local dir in ```awk```
```bash
# run the 'run.sh' script from subdirs having 'scripts' in their name
ls | grep scripts | awk ' { print "cd " ENVIRON["PWD"] "/" $1 " && ../run.sh"} '
```
### Binary file manipulation
#### Print the file bytes 
Use the ```xxd``` command
```bash
xxd -s 0 -c 4 -l 8 ~/work/binary_file
00000000: 0000 3442  ..4B
00000004: 0000 4842  ..HB
```
Options: ```-s``` for bytes to skip; ```-c``` for columns number (bytes in each row); ```-l``` for length (bytes to print)
#### Copy bytes from one file to another
Use the ```dd``` command to copy 4 bytes from the beginning of one file to another
```bash
dd if=/home/user/work/base.fvecs of=/home/user/work/base1.fvecs skip=0 bs=4 count=1
```

Use the ```dd``` command to copy 512 bytes from one file and append them to another
```bash
dd skip=4 bs=128 count=4 if=/home/user/work/base.fvecs >> /home/user/work/base1.fvecs
```
Options: ```if``` for input file; ```of``` for output file; ```skip``` for bytes to skip; ```bs``` block size in bytes; ```count``` number of blocks to copy
