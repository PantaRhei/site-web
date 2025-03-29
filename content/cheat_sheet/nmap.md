+++
date = '2025-03-18T15:53:48+01:00'
draft = false
title = 'Nmap'
+++

My cheat sheet for Nmap.

Syntax:
```bash
nmap [Scan Type] [Options] {target specification}
```

`-sS` => TCP SYN (send just TCP SYN never completes the three-way handshake)
  Results:
  - Open ports: SYN/ACK
  - Closed ports: RST
  - Filtered ports: No response

`-iL host.lst`=> list of host


# ICMP echo requests
```bash
sudo nmap [network adr]/[netmask] -sn -oA tnet | grep for | cut -d" " -f5
```
Arguments (`-PE` => disable port scan (-sn), Nmap automatically ping scan with ICMP Echo Requests (-PE) ):
- `-sn` => Ping scan
- `-oA` => Output to all formats
- `grep for` => Filter the line with "for"
- `cut -d" " -f5` => Cut the fifth field with space delimiter

## Different probe types
`-PE` => ICMP echo requests
`-PS` => TCP SYN ping
`-PA` => TCP ACK ping
`-PU` => UDP ping
`-PO` => IP Protocol ping
`-PR` => ARP ping

## Other arg
- `--packet-trace` 	Shows all packets sent and received
- `--disable-arp-ping` 	Disables ARP ping
- `--reason` 	Displays the reason for specific result.

# Initial TTL Values

Exemple come from hack the box, we see the TTL value is 128 so a host is windows.
```bash
sudo nmap 10.129.2.18 -sn -oA host -PE --packet-trace --disable-arp-ping

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-15 00:12 CEST
SENT (0.0107s) ICMP [10.10.14.2 > 10.129.2.18 Echo request (type=8/code=0) id=13607 seq=0] IP [ttl=255 id=23541 iplen=28 ]
RCVD (0.0152s) ICMP [10.129.2.18 > 10.10.14.2 Echo reply (type=0/code=0) id=13607 seq=0] IP [ttl=128 id=40622 iplen=28 ]
Nmap scan report for 10.129.2.18
Host is up (0.086s latency).
MAC Address: DE:AD:00:00:BE:EF
Nmap done: 1 IP address (1 host up) scanned in 0.11 seconds
```

The following table shows the default Initial TTL values of various operating systems and devices.

| Device / OS       | Version                | Protocol       | TTL |
|-------------------|------------------------|----------------|-----|
| AIX               |                        | TCP            | 60  |
| AIX               |                        | UDP            | 30  |
| Android           | 3.2.1                  | TCP and ICMP   | 64  |
| Android           | 5.1.1                  | TCP and ICMP   | 64  |
| AIX               | 3.2, 4.1               | ICMP           | 255 |
| BSDI              | BSD/OS 3.1 and 4.0     | ICMP           | 255 |
| Compa             | Tru64 v5.0             | ICMP           | 64  |
| Cisco             |                        | ICMP           | 254 |
| DEC Pathworks     | V5                     | TCP and UDP    | 30  |
| Foundry           |                        | ICMP           | 64  |
| FreeBSD           | 2.1R                   | TCP and UDP    | 64  |
| FreeBSD           | 3.4, 4.0               | ICMP           | 255 |
| FreeBSD           | 5                      | ICMP           | 64  |
| HP-UX             | 9.0x                   | TCP and UDP    | 30  |
| HP-UX             | 10.01                  | TCP and UDP    | 64  |
| HP-UX             | 10.2                   | ICMP           | 255 |
| HP-UX             | 11                     | ICMP           | 255 |
| HP-UX             | 11                     | TCP            | 64  |
| Irix              | 5.3                    | TCP and UDP    | 60  |
| Irix              | 6.x                    | TCP and UDP    | 60  |
| Irix              | 6.5.3, 6.5.8           | ICMP           | 255 |
| Juniper           |                        | ICMP           | 64  |
| MPE/IX (HP)       |                        | ICMP           | 200 |
| Linux             | 2.0.x kernel           | ICMP           | 64  |
| Linux             | 2.2.14 kernel          | ICMP           | 255 |
| Linux             | 2.4 kernel             | ICMP           | 255 |
| Linux             | Red Hat 9              | ICMP and TCP   | 64  |
| MacOS/MacTCP      | 2.0.x                  | TCP and UDP    | 60  |
| MacOS/MacTCP      | X (10.5.6)             | ICMP/TCP/UDP   | 64  |
| NetBSD            |                        | ICMP           | 255 |
| Netgear FVG318    |                        | ICMP and UDP   | 64  |
| OpenBSD           | 2.6 & 2.7              | ICMP           | 255 |
| OpenVMS           | 07.01.2002             | ICMP           | 255 |
| OS/2              | TCP/IP 3.0             |                | 64  |
| OSF/1             | V3.2A                  | TCP            | 60  |
| OSF/1             | V3.2A                  | UDP            | 30  |
| Solaris           | 2.5.1, 2.6, 2.7, 2.8   | ICMP           | 255 |
| Solaris           | 2.8                    | TCP            | 64  |
| Stratus           | TCP_OS                 | ICMP           | 255 |
| Stratus           | TCP_OS (14.2-)         | TCP and UDP    | 30  |
| Stratus           | TCP_OS (14.3+)         | TCP and UDP    | 64  |
| Stratus           | STCP                   | ICMP/TCP/UDP   | 60  |
| SunOS             | 4.1.3/4.1.4            | TCP and UDP    | 60  |
| SunOS             | 5.7                    | ICMP and TCP   | 255 |
| Ultrix            | V4.1/V4.2A             | TCP            | 60  |
| Ultrix            | V4.1/V4.2A             | UDP            | 30  |
| Ultrix            | V4.2 – 4.5             | ICMP           | 255 |
| VMS/Multinet      |                        | TCP and UDP    | 64  |
| VMS/TCPware      |                        | TCP            | 60  |
| VMS/TCPware      |                        | UDP            | 64  |
| VMS/Wollongong   | 1.1.1.1                | TCP            | 128 |
| VMS/Wollongong   | 1.1.1.1                | UDP            | 30  |
| VMS/UCX           |                        | TCP and UDP    | 128 |
| Windows          | for Workgroups         | TCP and UDP    | 32  |
| Windows          | 95                     | TCP and UDP    | 32  |
| Windows          | 98                     | ICMP           | 32  |
| Windows          | 98, 98 SE              | ICMP           | 128 |
| Windows          | 98                     | TCP            | 128 |
| Windows          | NT 3.51                | TCP and UDP    | 32  |
| Windows          | NT 4.0                 | TCP and UDP    | 128 |
| Windows          | NT 4.0 SP5-            |                | 32  |
| Windows          | NT 4.0 SP6+            |                | 128 |
| Windows          | NT 4 WRKS SP 3, SP 6a  | ICMP           | 128 |
| Windows          | NT 4 Server SP4        | ICMP           | 128 |
| Windows          | ME                     | ICMP           | 128 |
| Windows          | 2000 pro               | ICMP/TCP/UDP   | 128 |
| Windows          | 2000 family            | ICMP           | 128 |
| Windows          | Server 2003            |                | 128 |
| Windows          | XP                     | ICMP/TCP/UDP   | 128 |
| Windows          | Vista                  | ICMP/TCP/UDP   | 128 |
| Windows          | 7                      | ICMP/TCP/UDP   | 128 |
| Windows          | Server 2008            | ICMP/TCP/UDP   | 128 |
| Windows          | 10                     | ICMP/TCP/UDP   | 128 |


# Port Scanning
## Port States ad Descriptions

| State           | Description                                                                                       |
|-----------------|---------------------------------------------------------------------------------------------------|
| open            | This indicates that the connection to the scanned port has been established. These connections can be TCP connections, UDP datagrams as well as SCTP associations. |
|-----------------|---------------------------------------------------------------------------------------------------|
| closed          | When the port is shown as closed, the TCP protocol indicates that the packet we received back contains an RST flag. This scanning method can also be used to determine if our target is alive or not. |
|-----------------|---------------------------------------------------------------------------------------------------|
| filtered        | Nmap cannot correctly identify whether the scanned port is open or closed because either no response is returned from the target for the port or we get an error code from the target. |
|-----------------|---------------------------------------------------------------------------------------------------|
| unfiltered      | This state of a port only occurs during the TCP-ACK scan and means that the port is accessible, but it cannot be determined whether it is open or closed. |
|-----------------|---------------------------------------------------------------------------------------------------|
| open\|filtered  | If we do not get a response for a specific port, Nmap will set it to that state. This indicates that a firewall or packet filter may protect the port. |
|-----------------|---------------------------------------------------------------------------------------------------|
| closed\|filtered| This state only occurs in the IP ID idle scans and indicates that it was impossible to determine if the scanned port is closed or filtered by a firewall. |
|-----------------|---------------------------------------------------------------------------------------------------|

## Filtered Ports
Tips:
- Check time to response (slow response => filtered port)
- Check with `--packet-trace` option to see the packets sent and received (error message => filtered port)


## Port Scan

```bash
sudo nmap [ip] -p [port] --packet-trace -Pn -n --disable-arp-ping
```

Arguments:
- `-p` => Port
- `--packet-trace` => Shows all packets sent and received
- `-Pn` => No ping
- `-n` => No DNS resolution (Performance, Privacy, Accuracy and Network Restrictions)
- `--disable-arp-ping` => Disables ARP ping

### TCP connect scan (-sT)
Exemple come to hack the box:
```bash
sudo nmap 10.129.2.28 -p 443 --packet-trace --disable-arp-ping -Pn -n --reason -sT
Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-15 16:26 CET
CONN (0.0385s) TCP localhost > 10.129.2.28:443 => Operation now in progress
CONN (0.0396s) TCP localhost > 10.129.2.28:443 => Connected
Nmap scan report for 10.129.2.28
Host is up, received user-set (0.013s latency).

PORT    STATE SERVICE REASON
443/tcp open  https   syn-ack

Nmap done: 1 IP address (1 host up) scanned in 0.04 seconds
```



### UDP Scan (-sU)
Exemple to UDP scan:
```bash
sudo nmap [ip] -F -sU

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-15 16:01 CEST
Nmap scan report for [ip]
Host is up (0.059s latency).
Not shown: 95 closed ports
PORT     STATE         SERVICE
68/udp   open|filtered dhcpc
137/udp  open          netbios-ns
138/udp  open|filtered netbios-dgm
631/udp  open|filtered ipp
5353/udp open          zeroconf
MAC Address: [mac addr] ([constructor])

Nmap done: 1 IP address (1 host up) scanned in 98.07 seconds
```
With option `--pack-trace`:
- `open` => if we get a response
- `closed` => ICMP response with error code 3 (port unreachable)
- `open|filtered` => for all other ICMP responses


### Version Scan (-sV)
```bash
sudo nmap [ip] -Pn -n --disable-arp-ping --packet-trace -p [port] --reason  -sV
```

# Saving data
- Normal output (`-oN`) with the `.nmap` file extension
- Grepable output (`-oG`) with the `.gnmap` file extension
- XML output (`-oX`) with the `.xml` file extension

For convert XML to HTML:
```bash
xsltproc target.xml -o target.html
```

# Nmap Scripting Engine



| Category   | Description                                                                 |
|------------|-----------------------------------------------------------------------------|
| auth       | Determination of authentication credentials.                                |
| broadcast  | Scripts, which are used for host discovery by broadcasting and the discovered hosts, can be automatically added to the remaining scans. |
| brute      | Executes scripts that try to log in to the respective service by brute-forcing with credentials. |
| default    | Default scripts executed by using the -sC option.                           |
| discovery  | Evaluation of accessible services.                                          |
| dos        | These scripts are used to check services for denial of service vulnerabilities and are used less as it harms the services. |
| exploit    | This category of scripts tries to exploit known vulnerabilities for the scanned port. |
| external   | Scripts that use external services for further processing.                 |
| fuzzer     | This uses scripts to identify vulnerabilities and unexpected packet handling by sending different fields, which can take much time. |
| intrusive  | Intrusive scripts that could negatively affect the target system.          |
| malware    | Checks if some malware infects the target system.                          |
| safe       | Defensive scripts that do not perform intrusive and destructive access.    |
| version    | Extension for service detection.                                           |
| vuln       | Identification of specific vulnerabilities.                                |

```bash
sudo nmap <target> --script <script-name>,<script-name>,...
```

Usefull scripts:
```bash
sudo nmap <target> --script vuln
```
Tip for my self: look at in `robot.txt`

Exemple come from hack the box:
```bash
sudo nmap 10.129.2.28 -p 80 -sV --script vuln

Nmap scan report for 10.129.2.28
Host is up (0.036s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
| http-enum:
|   /wp-login.php: Possible admin folder
|   /readme.html: Wordpress version: 2
|   /: WordPress version: 5.3.4
|   /wp-includes/images/rss.png: Wordpress version 2.2 found.
|   /wp-includes/js/jquery/suggest.js: Wordpress version 2.5 found.
|   /wp-includes/images/blank.gif: Wordpress version 2.6 found.
|   /wp-includes/js/comment-reply.js: Wordpress version 2.7 found.
|   /wp-login.php: Wordpress login page.
|   /wp-admin/upgrade.php: Wordpress login page.
|_  /readme.html: Interesting, a readme.
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
| http-wordpress-users:
| Username found: admin
|_Search stopped at ID #25. Increase the upper limit if necessary with 'http-wordpress-users.limit'
| vulners:
|   cpe:/a:apache:http_server:2.4.29:
|     	CVE-2019-0211	7.2	https://vulners.com/cve/CVE-2019-0211
|     	CVE-2018-1312	6.8	https://vulners.com/cve/CVE-2018-1312
|     	CVE-2017-15715	6.8	https://vulners.com/cve/CVE-2017-15715
<SNIP>
```



# Interesting flags
- `--stats-every=5s` 	Shows the progress of the scan every 5 seconds.

# Reference
- [nmap man host discovery strat](https://nmap.org/book/host-discovery-strategies.html)
- [scan methods connect scan](https://nmap.org/book/scan-methods-connect-scan.html)
- [man port scanning techniques](https://nmap.org/book/man-port-scanning-techniques.html)
- [nmap format output](https://nmap.org/book/output.html)
- [Nmap Scripting Engine (NSE)](https://nmap.org/nsedoc/index.html)
- [Performance timing templates](https://nmap.org/book/performance-timing-templates.html)
