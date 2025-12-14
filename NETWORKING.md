# Networking Cheat Sheet

## Files in Linix
```/etc/nsswitch.conf``` - Defines the order of domain name resolution (what comes fitst, e.g.: hosts and then dns or )

## Commands
### DNS
```$ nslookup``` - Query domain name for IP address
```$ cat /etc/resolv.conf```
### Routing
```$ ip route add 192.168.1.0/24 via 192.168.2.1```

### Handy Commands for Network Debugging

```
$ ip link
```
```
$ ip addr
```
```
$ ip addr add 192.168.1.0/24 dev eth0
```
```
$ ip route
```
```
$ route
```
```
$ cat /proc/sys/net/ipv4/ip_forward
```
```
$ arp
```
```
$ netstat -plnt
```
### NFS Volume Mounting
#### ✅ On NFS server
1. Make sure NFS is installed
```bash
sudo apt update
sudo apt install nfs-kernel-server
```
2. Export the existing volume /NVME1
Edit exports. Run:
```bash
echo "/mnt/NVME1/   *(rw,sync,no_subtree_check)" | sudo tee -a /etc/exports
```
For mapping to a specified client run:
```bash
echo "/mnt/NVME1/   <NFS CLIENT IP>(rw,sync,no_subtree_check)" | sudo tee -a /etc/exports
```
3. Apply the changes:
```bash
sudo exportfs -ra
sudo systemctl restart nfs-kernel-server
```
Make sure the changes are effective:
```bash
showmount -e
```


#### 🔵 On NFS client
1. Make sure NFS client is installed
```bash
sudo apt install nfs-common
```
3. Create a mount point
```bash
sudo mkdir -p /mnt/NVME1/
```
5. Mount the exported volume
```bash
sudo mount <NFS Server IP>:/mnt/NVME1/  /mnt/NVME1/
```
6. Test:
```bash
ls /mnt/NVME1/
```
7. Optional: Make the mount persist after reboot
Edit ```/etc/fstab```:
```bash
echo <NFS Server IP>:/mnt/NVME1/   /mnt/NVME1/   nfs   defaults   0  0 | sudo tee -a /etc/fstab
```



### Utils
[tcpdump](https://www.tcpdump.org/manpages/tcpdump.1.html)

[dig - DNS lookup utility](https://linux.die.net/man/1/dig)

#### Loop until a service becomes available
```bash
until nc -z localhost 27017
do
    sleep 1
done
```
##### This loop runs until a MongoDB database becomes available on port 27017 of localhost
* **until** - This loop will keep running until the command succeeds.
* **nc** - This stands for Netcat, a utility for network debugging and testing.
* **-z** - This option tells nc to scan for open ports without sending any data.
* The loop runs while the nc command fails (i.e., the port is not open).
