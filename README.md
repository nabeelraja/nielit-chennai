# NIELIT CHENNAI CLOUD COMPUTING

## DNS


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
5. Add yahoo.com
6. Update the list to add blocked phrase
```
vim /etc/dansguardian/lists/bannedsitelist
vim /etc/dansguardian/lists/bannedphraselist
```
7. Add <yahoo>
8. Restart Dansguardian service
```
service dansguardian restart
```
9. Update proxy settings in browser
```
<IP_ADDRESS>:8080
```
