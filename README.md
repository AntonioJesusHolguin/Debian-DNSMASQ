# Debian-DNSMASQ

dnsmasq is free software providing Domain Name System (DNS) caching, a Dynamic Host Configuration Protocol (DHCP) server, router advertisement and network boot features, intended for small computer networks.

dnsmasq has low requirements for system resources, can run on Linux, BSDs, Android and macOS, and is included in most Linux distributions. Consequently, it "is present in a lot of home routers and certain Internet of Things gadgets"[4] and is included in Android.

## Index

1. [What's dnsmasq?](#what)
2. [How to install dnsmasq](#install)
3. [How to configure dnsmasq](#config)
4. [How does it work?](#work)
5. [References](#references)

<a name="what"></a>
## 1.- What's dnsmasq?
dnsmasq (short for DNS masquerade) is a lightweight, easy to configure DNS forwarder, designed to provide DNS (and optionally DHCP and TFTP) services to a small-scale network. It can serve the names of local machines which are not in the global DNS.

dnsmasq's DHCP server supports static and dynamic DHCP leases, multiple networks and IP address ranges. The DHCP server integrates with the DNS server and allows local machines with DHCP-allocated addresses to appear in the DNS. dnsmasq caches DNS records, reducing the load on upstream nameservers and improving performance, and can be configured to automatically pick up the addresses of its upstream servers.

dnsmasq accepts DNS queries and either answers them from a small, local cache or forwards them to a real, recursive DNS server. It loads the contents of /etc/hosts, so that local host names which do not appear in the global DNS can be resolved. This also means that records added to your local /etc/hosts file with the format "0.0.0.0 annoyingsite.com" can be used to prevent references to "annoyingsite.com" from being resolved by your browser. This can quickly evolve to a local ad blocker when combined with adblocking site list providers. If done on a router, one can efficiently remove advertising content for an entire household or company.

<a name="install"></a>
## 2.- How to install dnsmasq
Now we now what's dnsmasq. Type this command to download and install dnsmasq:

```
$ apt -y install dnsmasq resolvconf
```

<a name="config"></a>
## 3.- How to configure dnsmasq
We can edit dnsmasq in /etc/dnsmasq.conf:

```
$ vi /etc/dnsmasq.conf

# line 19: uncomment (never forward plain names)
domain-needed
# line 21: uncomment (never forward addresses in the non-routed address spaces)
bogus-priv
# line 53: uncomment (query with each server strictly in the order in resolv.conf)
strict-order
# line 67: add if you need
# query the specific domain name to the specific DNS server
# the example follows means query [server.education] domain to the [10.0.0.10] server
server=/server.education/10.0.0.10
# line 135: uncomment (add domain name automatically)
expand-hosts
# line 145: add (define domain name)
domain=srv.world
```

To apply these effects we have to restart dnsmasq's service:

```
$ systemctl restart dnsmasq
```

For DNS records, add them in [/etc/hosts]. Then, Dnsmasq will answer to queries from clients:

```
$ vi /etc/hosts

# add records
10.0.0.30       dlp.srv.world dlp
```

Remember to restart the service:

```
$ systemctl restart dnsmasq
```

Verify to resolve Name or IP address from a client computer in internal network. By the way, when Dnsmasq is running, fixed value [127.0.0.1] is added in [/etc/resolv.conf] and also the value of "dns-nameservers" in [/etc/network/interfaces] is added and managed in [/var/run/dnsmasq/resolv.conf].

```
$ vi /etc/network/interfaces

# change DNS setting to Dnsmasq Server
dns-nameservers 10.0.0.30
root@desktop:~# systemctl restart ifup@ens2 resolvconf
root@desktop:~# dig dlp.srv.world.

; <<>> DiG 9.11.5-P4-5.1-Debian <<>> dlp.srv.world.
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 37213
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;dlp.srv.world.                 IN      A

;; ANSWER SECTION:
dlp.srv.world.          0       IN      A       10.0.0.30

;; Query time: 0 msec
;; SERVER: 127.0.0.1#53(127.0.0.1)
;; WHEN: Tue Jul 16 19:52:04 JST 2019
;; MSG SIZE  rcvd: 58

root@desktop:~# dig -x 10.0.0.30

; <<>> DiG 9.11.5-P4-5.1-Debian <<>> -x 10.0.0.30
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 12621
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;30.0.0.10.in-addr.arpa.                IN      PTR

;; ANSWER SECTION:
30.0.0.10.in-addr.arpa. 0       IN      PTR     dlp.srv.world.

;; Query time: 0 msec
;; SERVER: 127.0.0.1#53(127.0.0.1)
;; WHEN: Tue Jul 16 13:52:54 JST 2019
;; MSG SIZE  rcvd: 78
```
<a name="work"></a>
## 4.- How does it work?
After taking a closer look to study dnsmasq's files and how to configure them, let's create our own dns service step by step:

1.-



2.-



3.-



4.-



5.-



6.-

<a name="references"></a>
## 5.- References
- [dnsmasq's wikipedia page](https://en.wikipedia.org/wiki/Dnsmasq#:~:text=dnsmasq%20(short%20for%20DNS%20masquerade,not%20in%20the%20global%20DNS.)
- [How to install dnsmasq on Debian 10](https://www.server-world.info/en/note?os=Debian_10&p=dnsmasq&f=1)
