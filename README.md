# NIELIT CHENNAI CLOUD COMPUTING

## DNS


## Webserver


## Squid Proxy

1. Install package
```sh
yum install squid
```
2. Edit config file
```
vim /etc/squid/squid.conf
```
3. Add rules
```
acl local1 src <IP_ADDRESS>
acl fb dstdomain www.facebook.com
acl off_time time SMTWHFA 09:00-17:00
http_reply_access deny fb local1 office_time
```
4. Configure proxy in browser
```
<IP_ADDRESS>:3128
```
5. Restart squid service
```
service restart squid
```
