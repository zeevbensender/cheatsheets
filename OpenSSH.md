# OpenSSH

 
## Installing OpenSSH (client and server)
1. Ubuntu
```
$ sudo apt update && sudo apt install openssh-server openssh-client
```
 
2. CentOS
```
$ sudo dnf install openssh-server openssh-clients
```
 
## Connecting to the server
```
$ ssh -p 22 username@server_ip
```
```
$ ssh -p 22 -l username server_ip
```
Verbose:
```
$ ssh -v -p 22 username@server_ip
```
 
## Controlling the SSHd daemon
### Checking status
Ubuntu
```
$ sudo systemctl status ssh 
```
CentOS
```
sudo systemctl status sshd
 ```
### Stopping the daemon
Ubuntu
```
$ sudo systemctl stop ssh
```
CentOS
```
sudo systemctl stop sshd
``` 
### Restarting the daemon
Ubuntu
```
$ sudo systemctl restart ssh       # => Ubuntu
$ sudo systemctl restart sshd      # => CentOS
```
# Enabling at boot time 
Ubuntu
```
$ sudo systemctl is-enabled ssh
$ sudo systemctl enable ssh
```
CentOS
```
$ sudo systemctl enable sshd
$ sudo systemctl is-enabled sshd
```
## Securing the SSHd daemon
change the configuration file ```/etc/ssh/sshd_config``` and then restart the server
```
$ man sshd_config
 ```
1. Change the port
```
Port 2278
```
2. Disable direct root login
```
PermitRootLogin no
```
3. Limit Users’ SSH access
```
AllowUsers lupo user2 user3
```
4. Filter SSH access at the firewall level ```iptables```
 
5. Activate Public Key Authentication and Disable Password Authentication
 
6. Use only SSH Protocol version 2
 
7. Other configurations:
```
ClientAliveInterval 300
ClientAliveCountMax 0
MaxAuthTries 2
MaxStartUps 3
LoginGraceTime 20
```