# NIELIT CHENNAI CLOUD COMPUTING

## TOC
- [Pre-requisites](#pre-requisites)
- [DNS](#dns)
- [Webserver](#webserver)
- [Squid Proxy](#squid-proxy)
- [Dansguardian](#dansguardian)

## Pre-requisites

1. Add a new CentOS-6 VM
2. Open terminal as root user
```
su -
```
3. Get current ipaddress of the machine
```
ifconfig
```
4. Open network settings, delete eth1 interface and change from DHCP to static IP (current IP address)
IP Address: <IP_ADDRESS>
Netmask: 255.255.252.0
Gateway: 192.168.120.0
5. Restart network service
```
service network restart
```
6. Check firewall status and disable firewall if active
```
service iptables status
service ip6tables status

service iptables stop
service ip6tables stop

chkconfig iptables off
chkconfig ip6tables off
```
7. Check and disable SELINUX if active
```
getenforce

vim /etc/sysconfig/selinux

Ensure SELINUX=disabled
```
8. Change default server name
```
vim /etc/sysconfig/network

HOSTNAME=<SERVER_NAME>
```
9. Add hostname to host file
```
vim /etc/hosts

<IP_ADDRESS> <SERVER_NAME>
```
10. Reboot the machine
```
reboot
```
11. Validate if the above settings are correct.

## DNS
1. Install package
```
yum install bind*
```
2. Take a backup of named.conf file
```
cp /etc/named.conf /etc/named.conf.BAK
```  
3. Make the following changes to the named.conf file
    1. listen-on: add <IP_ADDRESS>;
    2. disable listen-on-v6
    3. allow query: add 0.0.0.0/0
    4. disable recursion
    5. disable security
    6. Create new entry for zone:
        1. type: master
        2. file: /etc/named/<ZONE_FILE_NAME>
        ```
        zone "nabeel.com" IN {
          type: master
          file: "/etc/named/nabeel.com.zone"
        }
        ```
4. Validate named.conf configuration
```
named-checkconf /etc/named.conf
```
5. Create zone file
```
vim /etc/named/<ZONE_FILE_NAME>
```
6. Enter the following details:
    1. SOA
    2. NS
    3. MX
    4. A
    5. CNAME
    ```
    $TTL 6H
    @	IN	SOA	nabeel.com. root.nabeel.com. ( 0 1H 2H 1W 5H )
    @	IN	MX	10 mail.nabeel.com.
    @	IN	NS	ns1.nabeel.com.
    ns1	IN	A	<IP_ADDRESS>
    www	IN	A	<IP_ADDRESS>
    ftp	IN	CNAME	www
    ```

## Webserver


## Squid Proxy

1. Install package
```
yum install squid
```
2. Edit config file
```
vim /etc/squid/squid.conf
```
3. Add rules
```
acl local1 src <IP_ADDRESS>/<NET_MASK>
acl fb dstdomain .facebook.com
acl office_time time SMTWHFA 09:00-17:00
http_reply_access deny fb local1 office_time
```
4. Configure proxy in browser
```
<IP_ADDRESS>:3128
```
5. Restart squid service
```
service squid restart
```
6. Open browser and access www.facebook.com

## Dansguardian
1. Install package
```
yum install dansguardian
```
2. Edit config file
```
vim /etc/dansguardian/dansguardian.conf
```
3. Update proxyip
```
proxyip=<IP_ADDRESS>
```
4. Update the list to add blocked URL
```
vim /etc/dansguardian/lists/bannedsitelist
```
5. Add site to be blocked
```
yahoo.com
```
6. Update the list to add blocked phrase
```
vim /etc/dansguardian/lists/bannedphraselist
```
7. Add phrase to be blocked
```
<yahoo>
```
8. Restart Dansguardian service
```
service dansguardian restart
```
9. Update proxy settings in browser
```
<IP_ADDRESS>:8080
```
