# Networking Cheat Sheet

## Files in Linix
```/etc/nsswitch.conf``` - Defines the order of domain name resolution (what comes fitst, e.g.: hosts and then dns or )

## Commands
### DNS
```$ nslookup``` - Query domain name for IP address
```$ cat /etc/resolv.conf```
### Routing
```$ ip route add 192.168.1.0/24 via 192.168.2.1```

### Handy Commands for k8s Network Debugging
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
