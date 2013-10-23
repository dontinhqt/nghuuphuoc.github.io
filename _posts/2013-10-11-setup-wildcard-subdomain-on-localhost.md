---
layout: post
title:  "Setup wildcard subdomain on localhost"
date:   2013-10-11 12:22:06
---

This article shows step-by-step instructions to setup ```*.domain.local``` domains on local environment.

On MacOS, it is NOT possible to add wildcard subdomains using ```/etc/hosts``` file as below:

```
127.0.0.1 *.domain.local
```

* Execute the following command:

```bash
$ sudo rndc-confgen -a -c /etc/rndc.key
wrote key file "/etc/rndc.key"
```
* Open ```/etc/named.conf``` and add the following block:

```
zone "local" IN {
    type master;
    file "local.zone";
    allow-update { none; };
};
```

* Create a file named ```local.zone``` located at ```/var/named``` with the following content:

```
$TTL  60
$ORIGIN local.
@      1D IN SOA  localhost. root.localhost. (
        45    ; serial (d. adams)
        3H    ; refresh
        15M   ; retry
        1W    ; expiry
        1D )  ; minimum

        1D IN NS  localhost.
        1D IN A  127.0.0.1

*.local. 60 IN A 127.0.0.1
domain.local. 60 IN A 127.0.0.1
*.domain.local. 60 IN A 127.0.0.1
```

* Check the syntax of config and zone files:

```bash
$ sudo named-checkconf /etc/named.conf
$ sudo named-checkzone local /var/named/local.zone
zone local/IN: loaded serial 45
OK
```

* Startup Bind by adding it to the Launch Daemons:

```bash
$ sudo launchctl load -w /System/Library/LaunchDaemons/org.isc.named.plist
```

Now, you can check if _named_ is started by _launchd_ by running the command:

```bash
$ ps aux | grep named
phuoc   1822   0.0  0.0  2432768    596 s001  R+    4:31PM   0:00.00 grep named
root    1819   0.0  0.1  2438720   6316   ??  Ss    4:31PM   0:00.04 /usr/sbin/named -f
```

* Verify all the setups work:

```bash
$ dig @localhost domain.local

; <<>> DiG 9.8.3-P1 <<>> @localhost domain.local
; (3 servers found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 29595
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 1, ADDITIONAL: 1

;; QUESTION SECTION:
;domain.local.			IN	A

;; ANSWER SECTION:

domain.local.		60	IN	A	127.0.0.1

;; AUTHORITY SECTION:
local.			86400	IN	NS	localhost.

;; ADDITIONAL SECTION:
localhost.		86400	IN	A	127.0.0.1

;; Query time: 0 msec
;; SERVER: 127.0.0.1#53(127.0.0.1)
;; WHEN: Thu May 23 13:47:28 2013
;; MSG SIZE  rcvd: 85
```

```bash
$ dig @localhost sub.domain.local

; <<>> DiG 9.8.3-P1 <<>> @localhost sub.domain.local
; (3 servers found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 28961
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 1, ADDITIONAL: 1

;; QUESTION SECTION:
;sub.domain.local.		IN	A

;; ANSWER SECTION:

sub.domain.local.	60	IN	A	127.0.0.1

;; AUTHORITY SECTION:
local.			86400	IN	NS	localhost.

;; ADDITIONAL SECTION:
localhost.		86400	IN	A	127.0.0.1

;; Query time: 0 msec
;; SERVER: 127.0.0.1#53(127.0.0.1)
;; WHEN: Thu May 23 13:48:13 2013
;; MSG SIZE  rcvd: 89
```

In the _ANSWER SECTION_ of each response, we can see that the ```domain.local``` and ```sub.domain.local``` domains are resolved to 127.0.0.1.

* Add 127.0.0.1 to list of DNS server:

	- Open _System Preferences &rarr; Network_
	- On the left side, choose the current network, and then click _Advanced_
  	- Under the _DNS_ tab, in the _DNS Servers_ list, click the + button and add ```127.0.0.1```
  	- Click _OK_, and then click _Apply_

![Configure DNS server](/img/modify-dns.png)

* Reload Bind:

```bash
$ sudo rndc -p 54 reload
server reload successful
```

In some cases, we need to flush DNS cache:

```bash
$ dscacheutil -flushcache
```

## UPDATE: Use dnsmasq as an alternative way

The following steps show you how to use ```dnsmasq``` to setup wildcard domain for localhost:

* Create ```local``` domain by adding to ```/etc/hosts``` the following line:

```
127.0.0.1  local
```

* Install dnsmasq and set it to launch at startup:

```bash
$ sudo port install dnsmasq
$ sudo sudo port load dnsmasq
```

* Open the dnsmasq configuration file which is located at ```/opt/local/etc/dnsmasq.conf``` and add the line:

```
address=/.local/127.0.0.1
```

You can check the local domain:

```bash
$ ping domain.local
$ dig @localhost domain.local

; <<>> DiG 9.8.3-P1 <<>> @localhost domain.local
; (3 servers found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 17555
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;domain.local.			IN	A

;; ANSWER SECTION:
domain.local.		0	IN	A	127.0.0.1

;; Query time: 0 msec
;; SERVER: ::1#53(::1)
;; WHEN: Thu Oct 24 00:04:43 2013
;; MSG SIZE  rcvd: 44
```

Check the sub domain:

```bash
$ ping sub.domain.local
$ dig @localhost sub.domain.local

; <<>> DiG 9.8.3-P1 <<>> @localhost sub.domain.local
; (3 servers found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 54492
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;sub.domain.local.			IN	A

;; ANSWER SECTION:
sub.domain.local.		0	IN	A	127.0.0.1

;; Query time: 0 msec
;; SERVER: ::1#53(::1)
;; WHEN: Thu Oct 24 00:05:24 2013
;; MSG SIZE  rcvd: 48
```