# Linux Security
## Password related
### File that stores user passwords
```
/etc/shadow
```
### Locking password authentication
```
$ sudo passwd -l USERNAME
$ sudo password --lock USERNAME
```
 
### Checking the account status
```
$ sudo passwd --status USERNAME
$ sudo chage -l USERNAME
``` 
### Unlocking password authentication
```
$ sudo passwd -u USERNAME
``` 
### Disable an account completely
```
$ sudo usermod --expiredate 1 lupo
$ sudo usermod --expiredate 1970-01-02 lupo
``` 
### Checking the account expiration date
```
$ sudo chage -l lupo
```

## sudo
### This file defines sudoer permissions
```
/etc/sudoers
```
## nmap
### Scan open ports on the host
```
$ nmap IP ADDRESS
```
### Scan all (not only standard) ports on the host
```
$ nmap -sS IP ADDRESS
```
### Scan open ports on the host and try to identify what app opened the port
```
$ nmap -V IP ADDRESS
```
### Scan open ports on the hosts of the network
```
$ nmap -ns 192.168.0.0/24
```
