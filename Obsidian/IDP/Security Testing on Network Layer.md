


# Nmap (https://nmap.org/)

Nmap, short for Network Mapper, is a powerful open-source network scanning tool used for discovering hosts and services on a computer network, thus creating a "map" of the network's topology. It employs various techniques like port scanning, OS detection, version detection, and scriptable interaction with the target systems. Nmap is widely utilized by network administrators, security professionals, and hackers alike for network inventory, managing service upgrade schedules, and vulnerability assessment.

| Port | Service  | Wanted                |
| ---- | -------- | --------------------- |
| 22   | SSH      | yes                   |
| 80   | HTTP     | yes, FastAPI          |
| 443  | HTTPS    | yes, FastAPI with TLS |
| 8086 | InfluxDB | yes                   |

The system in question comprises a server operating on an Ubuntu operating system. Running on this Ubuntu server is Nginx version 1.18.0, which serves as the web server for the FastAPI. Version 1.18.0 suggests that it is a stable release and suitable for production environments. Additionally, the system hosts an instance of InfluxDB version 2.7.4, running on port 8086. The specific use of port 8086 is standard for InfluxDB.

```
# Nmap 7.80 scan initiated Thu Apr 18 16:09:01 2024 as: nmap -sS -sU -vvv -A -sV -p- -Pn -oN scan_result.txt api2.eversion.tech
Nmap scan report for api2.eversion.tech (194.164.55.211)
Host is up, received user-set (0.021s latency).
Scanned at 2024-04-18 16:09:02 CEST for 454482s
Not shown: 130990 closed ports
Reason: 65526 resets and 65464 port-unreaches
PORT      STATE         SERVICE      REASON                                      VERSION
22/tcp    open          ssh          syn-ack ttl 53                              OpenSSH 8.9p1 Ubuntu 3ubuntu0.6 (Ubuntu Linux; protocol 2.0)
80/tcp    open          http         syn-ack ttl 55                              nginx 1.18.0 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to https://api2.eversion.tech/
135/tcp   filtered      msrpc        no-response
137/tcp   filtered      netbios-ns   no-response
138/tcp   filtered      netbios-dgm  no-response
139/tcp   filtered      netbios-ssn  no-response
443/tcp   open          ssl/http     syn-ack ttl 55                              nginx 1.18.0 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: Site doesn't have a title (application/json).
| ssl-cert: Subject: commonName=api2.eversion.tech
| Subject Alternative Name: DNS:api2.eversion.tech
| Issuer: commonName=R3/organizationName=Let's Encrypt/countryName=US
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-02-27T23:12:54
| Not valid after:  2024-05-27T23:12:53
| MD5:   eba9 fea9 2fff cb72 735c c58c cda4 1f4a
| SHA-1: dd20 0951 bd63 9ee3 f87c 8029 714c 314e 9592 5321
| -----BEGIN CERTIFICATE-----
| MIIE8jCCA9qgAwIBAgISA3pM83kKW7HTmtDmcMGe6HbHMA0GCSqGSIb3DQEBCwUA
| MDIxCzAJBgNVBAYTAlVTMRYwFAYDVQQKEw1MZXQncyBFbmNyeXB0MQswCQYDVQQD
| EwJSMzAeFw0yNDAyMjcyMzEyNTRaFw0yNDA1MjcyMzEyNTNaMB0xGzAZBgNVBAMT
| EmFwaTIuZXZlcnNpb24udGVjaDCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoC
| ggEBAMsoZYOflMObkBFYipx9Atek2xr+mthpBGXia0mYo9CQV0QITMNCGikRTDLz
| qS10YDCPUj8tCLOa4u+TUYqAYdkYCnBstz97c7f4NKr3jZD7R9nB+Cvpb6t5k0es
| OWe+2u39UTjuW/+PxTYDB+nkCBzaARkPPP1cDUCGnxpA7xf13G0MhRW1Ky4FqhB5
| WayL8Bdjiv0XnBDKw+Fgr388bH9/mIRAaZfplIYkej+9oC6ERRPQAJaH4Cd/tQ+G
| i8sLkBbkXzzMuATXZJ7iaI3vEbqRp1U9ThJVIi7n5FMvLq1gWE0kSUFEX2jPaiu1
| PGkIWnCS/d7BYuLuhj9qfVQIk50CAwEAAaOCAhUwggIRMA4GA1UdDwEB/wQEAwIF
| oDAdBgNVHSUEFjAUBggrBgEFBQcDAQYIKwYBBQUHAwIwDAYDVR0TAQH/BAIwADAd
| BgNVHQ4EFgQULmAKi460PQ/y8eUqt261Z5EgCgowHwYDVR0jBBgwFoAUFC6zF7dY
| VsuuUAlA5h+vnYsUwsYwVQYIKwYBBQUHAQEESTBHMCEGCCsGAQUFBzABhhVodHRw
| Oi8vcjMuby5sZW5jci5vcmcwIgYIKwYBBQUHMAKGFmh0dHA6Ly9yMy5pLmxlbmNy
| Lm9yZy8wHQYDVR0RBBYwFIISYXBpMi5ldmVyc2lvbi50ZWNoMBMGA1UdIAQMMAow
| CAYGZ4EMAQIBMIIBBQYKKwYBBAHWeQIEAgSB9gSB8wDxAHcASLDja9qmRzQP5WoC
| +p0w6xxSActW3SyB2bu/qznYhHMAAAGN7Q6eQwAABAMASDBGAiEAo1IWzuuvn6gu
| R5tY6cwNlFM+SVpHQ9tfGg/ZfCevBlgCIQDZN2SQWNGds8vPdoRVQKJUvQ7jfvxs
| +l1OjiV6tcHOwQB2AHb/iD8KtvuVUcJhzPWHujS0pM27KdxoQgqf5mdMWjp0AAAB
| je0OoK4AAAQDAEcwRQIgVqSb4q7uEnT2yOGV65edPUOcznL7Dhp3GonK8lnBgIgC
| IQDO+D+dW/C99eU9m82TX3gGr/5zKwNjIiyoQ9jUORq1NjANBgkqhkiG9w0BAQsF
| AAOCAQEAgIkE1/9PQMeSP0lYWEVwIAbNPKzFNPrp5Oo43s82L6CMJAj422H+rnDh
| eTdFgWz3+wxRZiXtntxi9iOZZpO9LpQ9Wm5XFe3xv6zmU9gsIHWsLGhbGRr72nYO
| Mq3aoisre+Oek5heLzMVcZiFMyaV0F8tZLxPogAP7tKV24BSwLwOOvSA814bGmDK
| AJWSpId8QhJ9lAiM3LHQlZSM2yq5REW++GSuG3xGxmjjU2I35sYsjMfEkBTgXm9f
| qz0cIyPM+UefacK+ZeLOrR6lV92EVc+9TV9nC0I9IhxKmw0WX8c5EnrRtpT+EnDk
| 1/WJJ1leQGCURMHLY/C0qxLU5EOIPw==
|_-----END CERTIFICATE-----
445/tcp   filtered      microsoft-ds no-response
8086/tcp  open          ssl/d-s-n?   syn-ack ttl 54
| fingerprint-strings: 
|   GenericLines: 
|     HTTP/1.1 400 Bad Request
|     Content-Type: text/plain; charset=utf-8
|     Connection: close
|     Request
|   GetRequest, HTTPOptions: 
|     HTTP/1.0 200 OK
|     Accept-Ranges: bytes
|     Cache-Control: public, max-age=3600
|     Content-Length: 534
|     Content-Type: text/html; charset=utf-8
|     Etag: "5342613538"
|     Last-Modified: Wed, 26 Apr 2023 13:05:38 GMT
|     X-Influxdb-Build: OSS
|     X-Influxdb-Version: v2.7.4
|     Date: Tue, 23 Apr 2024 20:15:17 GMT
|_    <!doctype html><html lang="en"><head><meta charset="utf-8"><meta name="viewport" content="width=device-width,initial-scale=1"><meta name="description" content="InfluxDB is a time series platform, purpose-built by InfluxData for storing metrics and events, provides real-time visibility into stacks, sensors, and systems."><title>InfluxDB</title><base href="/"><base href=""><link rel="icon" href="/favicon.ico"></head><body><div id="react-root" data-basepath=""></div><script defer="defer" src="/fa4773d142.js"></script></body></html>
| ssl-cert: Subject: commonName=api2.eversion.tech
| Subject Alternative Name: DNS:api2.eversion.tech
| Issuer: commonName=R3/organizationName=Let's Encrypt/countryName=US
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-02-27T23:12:54
| Not valid after:  2024-05-27T23:12:53
| MD5:   eba9 fea9 2fff cb72 735c c58c cda4 1f4a
| SHA-1: dd20 0951 bd63 9ee3 f87c 8029 714c 314e 9592 5321
| -----BEGIN CERTIFICATE-----
| MIIE8jCCA9qgAwIBAgISA3pM83kKW7HTmtDmcMGe6HbHMA0GCSqGSIb3DQEBCwUA
| MDIxCzAJBgNVBAYTAlVTMRYwFAYDVQQKEw1MZXQncyBFbmNyeXB0MQswCQYDVQQD
| EwJSMzAeFw0yNDAyMjcyMzEyNTRaFw0yNDA1MjcyMzEyNTNaMB0xGzAZBgNVBAMT
| EmFwaTIuZXZlcnNpb24udGVjaDCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoC
| ggEBAMsoZYOflMObkBFYipx9Atek2xr+mthpBGXia0mYo9CQV0QITMNCGikRTDLz
| qS10YDCPUj8tCLOa4u+TUYqAYdkYCnBstz97c7f4NKr3jZD7R9nB+Cvpb6t5k0es
| OWe+2u39UTjuW/+PxTYDB+nkCBzaARkPPP1cDUCGnxpA7xf13G0MhRW1Ky4FqhB5
| WayL8Bdjiv0XnBDKw+Fgr388bH9/mIRAaZfplIYkej+9oC6ERRPQAJaH4Cd/tQ+G
| i8sLkBbkXzzMuATXZJ7iaI3vEbqRp1U9ThJVIi7n5FMvLq1gWE0kSUFEX2jPaiu1
| PGkIWnCS/d7BYuLuhj9qfVQIk50CAwEAAaOCAhUwggIRMA4GA1UdDwEB/wQEAwIF
| oDAdBgNVHSUEFjAUBggrBgEFBQcDAQYIKwYBBQUHAwIwDAYDVR0TAQH/BAIwADAd
| BgNVHQ4EFgQULmAKi460PQ/y8eUqt261Z5EgCgowHwYDVR0jBBgwFoAUFC6zF7dY
| VsuuUAlA5h+vnYsUwsYwVQYIKwYBBQUHAQEESTBHMCEGCCsGAQUFBzABhhVodHRw
| Oi8vcjMuby5sZW5jci5vcmcwIgYIKwYBBQUHMAKGFmh0dHA6Ly9yMy5pLmxlbmNy
| Lm9yZy8wHQYDVR0RBBYwFIISYXBpMi5ldmVyc2lvbi50ZWNoMBMGA1UdIAQMMAow
| CAYGZ4EMAQIBMIIBBQYKKwYBBAHWeQIEAgSB9gSB8wDxAHcASLDja9qmRzQP5WoC
| +p0w6xxSActW3SyB2bu/qznYhHMAAAGN7Q6eQwAABAMASDBGAiEAo1IWzuuvn6gu
| R5tY6cwNlFM+SVpHQ9tfGg/ZfCevBlgCIQDZN2SQWNGds8vPdoRVQKJUvQ7jfvxs
| +l1OjiV6tcHOwQB2AHb/iD8KtvuVUcJhzPWHujS0pM27KdxoQgqf5mdMWjp0AAAB
| je0OoK4AAAQDAEcwRQIgVqSb4q7uEnT2yOGV65edPUOcznL7Dhp3GonK8lnBgIgC
| IQDO+D+dW/C99eU9m82TX3gGr/5zKwNjIiyoQ9jUORq1NjANBgkqhkiG9w0BAQsF
| AAOCAQEAgIkE1/9PQMeSP0lYWEVwIAbNPKzFNPrp5Oo43s82L6CMJAj422H+rnDh
| eTdFgWz3+wxRZiXtntxi9iOZZpO9LpQ9Wm5XFe3xv6zmU9gsIHWsLGhbGRr72nYO
| Mq3aoisre+Oek5heLzMVcZiFMyaV0F8tZLxPogAP7tKV24BSwLwOOvSA814bGmDK
| AJWSpId8QhJ9lAiM3LHQlZSM2yq5REW++GSuG3xGxmjjU2I35sYsjMfEkBTgXm9f
| qz0cIyPM+UefacK+ZeLOrR6lV92EVc+9TV9nC0I9IhxKmw0WX8c5EnrRtpT+EnDk
| 1/WJJ1leQGCURMHLY/C0qxLU5EOIPw==
|_-----END CERTIFICATE-----
|_ssl-date: TLS randomness does not represent time
| tls-alpn: 
|   h2
|_  http/1.1
68/udp    open|filtered dhcpc        no-response
135/udp   open|filtered msrpc        no-response
137/udp   open|filtered netbios-ns   no-response
138/udp   open|filtered netbios-dgm  no-response
139/udp   open|filtered netbios-ssn  no-response
445/udp   open|filtered microsoft-ds no-response
499/udp   open|filtered iso-ill      no-response
2165/udp  open|filtered x-bone-api   no-response
3317/udp  open|filtered vsaiport     no-response
3544/udp  filtered      teredo       admin-prohibited from 217.226.46.215 ttl 63
3910/udp  open|filtered prnrequest   no-response
4791/udp  open|filtered unknown      no-response
4959/udp  open|filtered unknown      no-response
5413/udp  open|filtered wwiotalk     no-response
5478/udp  open|filtered unknown      no-response
8574/udp  open|filtered unknown      no-response
8722/udp  open|filtered unknown      no-response
9589/udp  open|filtered unknown      no-response
9817/udp  open|filtered unknown      no-response
10004/udp open|filtered unknown      no-response
10115/udp open|filtered netiq-endpt  no-response
10363/udp open|filtered unknown      no-response
10788/udp open|filtered unknown      no-response
11211/udp open|filtered memcache     no-response
11909/udp open|filtered unknown      no-response
13053/udp open|filtered unknown      no-response
13131/udp open|filtered unknown      no-response
14941/udp open|filtered unknown      no-response
15004/udp open|filtered unknown      no-response
15084/udp open|filtered unknown      no-response
16272/udp open|filtered unknown      no-response
18613/udp open|filtered unknown      no-response
18769/udp open|filtered ique         no-response
19192/udp open|filtered unknown      no-response
19795/udp open|filtered unknown      no-response
20594/udp open|filtered unknown      no-response
21465/udp open|filtered unknown      no-response
22048/udp open|filtered unknown      no-response
29311/udp open|filtered unknown      no-response
29392/udp open|filtered unknown      no-response
30777/udp open|filtered unknown      no-response
32652/udp open|filtered unknown      no-response
33573/udp open|filtered unknown      no-response
34180/udp open|filtered unknown      no-response
37434/udp open|filtered unknown      no-response
37851/udp open|filtered unknown      no-response
39212/udp open|filtered unknown      no-response
40704/udp open|filtered unknown      no-response
42342/udp open|filtered unknown      no-response
42770/udp open|filtered unknown      no-response
44408/udp open|filtered unknown      no-response
45931/udp open|filtered unknown      no-response
46368/udp open|filtered unknown      no-response
47478/udp open|filtered unknown      no-response
48671/udp open|filtered unknown      no-response
51463/udp open|filtered unknown      no-response
51770/udp open|filtered unknown      no-response
52767/udp open|filtered unknown      no-response
53942/udp open|filtered unknown      no-response
54404/udp open|filtered unknown      no-response
55442/udp open|filtered unknown      no-response
56042/udp open|filtered unknown      no-response
57798/udp open|filtered unknown      no-response
59070/udp open|filtered unknown      no-response
59436/udp open|filtered unknown      no-response
63617/udp open|filtered unknown      no-response
63949/udp open|filtered unknown      no-response
64390/udp open|filtered unknown      no-response
65166/udp open|filtered unknown      no-response
65356/udp open|filtered unknown      no-response
65395/udp open|filtered unknown      no-response
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port8086-TCP:V=7.80%T=SSL%I=7%D=4/23%Time=662816D5%P=x86_64-pc-linux-gn
SF:u%r(GenericLines,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type
SF::\x20text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x2
SF:0Bad\x20Request")%r(GetRequest,33B,"HTTP/1\.0\x20200\x20OK\r\nAccept-Ra
SF:nges:\x20bytes\r\nCache-Control:\x20public,\x20max-age=3600\r\nContent-
SF:Length:\x20534\r\nContent-Type:\x20text/html;\x20charset=utf-8\r\nEtag:
SF:\x20\"5342613538\"\r\nLast-Modified:\x20Wed,\x2026\x20Apr\x202023\x2013
SF::05:38\x20GMT\r\nX-Influxdb-Build:\x20OSS\r\nX-Influxdb-Version:\x20v2\
SF:.7\.4\r\nDate:\x20Tue,\x2023\x20Apr\x202024\x2020:15:17\x20GMT\r\n\r\n<
SF:!doctype\x20html><html\x20lang=\"en\"><head><meta\x20charset=\"utf-8\">
SF:<meta\x20name=\"viewport\"\x20content=\"width=device-width,initial-scal
SF:e=1\"><meta\x20name=\"description\"\x20content=\"InfluxDB\x20is\x20a\x2
SF:0time\x20series\x20platform,\x20purpose-built\x20by\x20InfluxData\x20fo
SF:r\x20storing\x20metrics\x20and\x20events,\x20provides\x20real-time\x20v
SF:isibility\x20into\x20stacks,\x20sensors,\x20and\x20systems\.\"><title>I
SF:nfluxDB</title><base\x20href=\"/\"><base\x20href=\"\"><link\x20rel=\"ic
SF:on\"\x20href=\"/favicon\.ico\"></head><body><div\x20id=\"react-root\"\x
SF:20data-basepath=\"\"></div><script\x20defer=\"defer\"\x20src=\"/fa4773d
SF:142\.js\"></script></body></html>")%r(HTTPOptions,33B,"HTTP/1\.0\x20200
SF:\x20OK\r\nAccept-Ranges:\x20bytes\r\nCache-Control:\x20public,\x20max-a
SF:ge=3600\r\nContent-Length:\x20534\r\nContent-Type:\x20text/html;\x20cha
SF:rset=utf-8\r\nEtag:\x20\"5342613538\"\r\nLast-Modified:\x20Wed,\x2026\x
SF:20Apr\x202023\x2013:05:38\x20GMT\r\nX-Influxdb-Build:\x20OSS\r\nX-Influ
SF:xdb-Version:\x20v2\.7\.4\r\nDate:\x20Tue,\x2023\x20Apr\x202024\x2020:15
SF::17\x20GMT\r\n\r\n<!doctype\x20html><html\x20lang=\"en\"><head><meta\x2
SF:0charset=\"utf-8\"><meta\x20name=\"viewport\"\x20content=\"width=device
SF:-width,initial-scale=1\"><meta\x20name=\"description\"\x20content=\"Inf
SF:luxDB\x20is\x20a\x20time\x20series\x20platform,\x20purpose-built\x20by\
SF:x20InfluxData\x20for\x20storing\x20metrics\x20and\x20events,\x20provide
SF:s\x20real-time\x20visibility\x20into\x20stacks,\x20sensors,\x20and\x20s
SF:ystems\.\"><title>InfluxDB</title><base\x20href=\"/\"><base\x20href=\"\
SF:"><link\x20rel=\"icon\"\x20href=\"/favicon\.ico\"></head><body><div\x20
SF:id=\"react-root\"\x20data-basepath=\"\"></div><script\x20defer=\"defer\
SF:"\x20src=\"/fa4773d142\.js\"></script></body></html>");
OS fingerprint not ideal because: Host distance (9 network hops) is greater than five
Aggressive OS guesses: Linux 2.6.32 (93%), Linux 3.2 - 4.9 (93%), Linux 2.6.32 - 3.10 (93%), Linux 3.4 - 3.10 (92%), Synology DiskStation Manager 5.2-5644 (91%), Netgear RAIDiator 4.2.28 (91%), Linux 3.1 (91%), Linux 3.2 (91%), Linux 2.6.32 - 2.6.35 (91%), Linux 2.6.32 - 3.5 (91%)
No exact OS matches for host (test conditions non-ideal).
TCP/IP fingerprint:
SCAN(V=7.80%E=4%D=4/23%OT=22%CT=1%CU=1%PV=N%DS=9%DC=T%G=N%TM=662818D1%P=x86_64-pc-linux-gnu)
SEQ(SP=103%GCD=1%ISR=109%TI=Z%CI=Z%II=I%TS=A)
OPS(O1=M5ACST11NW7%O2=M5ACST11NW7%O3=M5ACNNT11NW7%O4=M5ACST11NW7%O5=M5ACST11NW7%O6=M5ACST11)
WIN(W1=FE88%W2=FE88%W3=FE88%W4=FE88%W5=FE88%W6=FE88)
ECN(R=Y%DF=Y%T=3D%W=FAF0%O=M5ACNNSNW7%CC=Y%Q=)
T1(R=Y%DF=Y%T=3D%S=O%A=S+%F=AS%RD=0%Q=)
T2(R=N)
T3(R=N)
T4(R=Y%DF=Y%T=3E%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)
T5(R=Y%DF=Y%T=3F%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)
T6(R=Y%DF=Y%T=3D%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)
T7(R=Y%DF=Y%T=3F%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)
U1(R=Y%DF=N%T=3F%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=DE7F%RUD=G)
IE(R=Y%DFI=N%T=3F%CD=S)

Uptime guess: 14.214 days (since Tue Apr  9 17:15:23 2024)
Network Distance: 9 hops
TCP Sequence Prediction: Difficulty=260 (Good luck!)
IP ID Sequence Generation: All zeros
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 3389/tcp)
HOP RTT      ADDRESS
1   9.26 ms  fritz.box (192.168.178.1)
2   14.10 ms p3e9bf724.dip0.t-ipconnect.de (62.155.247.36)
3   28.19 ms b-ec8-i.B.DE.NET.DTAG.DE (217.5.75.218)
4   25.90 ms 62.157.249.142
5   25.91 ms lo-0-0.rc-a.rs.ber.de.net.ionos.com (212.227.117.204)
6   24.59 ms 212.227.120.165
7   ... 8
9   29.39 ms api2.eversion.tech (194.164.55.211)

Read data files from: /usr/bin/../share/nmap
OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Tue Apr 23 22:23:45 2024 -- 1 IP address (1 host up) scanned in 454483.25 seconds
```



# Metasploit

Metasploit is a powerful and widely-used framework for penetration testing and security auditing. It provides users with the tools to develop and execute exploit code against a remote target machine. The framework includes a vast library of publicly known exploits, which can be used to test security vulnerabilities in networks, servers, and applications. Metasploit is valuable not only for professional penetration testers but also for security researchers and systems administrators. It supports testing in various environments, thereby helping in identifying security weaknesses before they can be exploited maliciously. The framework's modular setup allows for the addition of custom exploits and includes features for evading detection, making it a comprehensive tool for improving system security.

The modules listed below are utilized for testing the InfluxDB and Nginx server:

- auxiliary/scanner/http/influxdb_enum
![[Pasted image 20240430090735.png]]

- exploit/linux/http/glinet_unauth_rce_cve_2023_50445  2023-12-10 with `Linux Dropper` as target
![[Pasted image 20240430085049.png]]

- exploit/linux/http/nginx_chunked_size with `Ubuntu 13.04 32bit - nginx 1.4.0`as target
![[Pasted image 20240430090628.png]]

- auxiliary/scanner/http/nginx_source_disclosure
![[Pasted image 20240430090553.png]]

- exploit/linux/http/roxy_wi_exec with `Linux (Dropper)`as target
![[Pasted image 20240430090453.png]]

- exploit/multi/http/php_fpm_rce with `PHP`as target
 ![[Pasted image 20240430090940.png]]

- exploit/multi/http/php_fpm_rce with `Shell Command`as target
![[Pasted image 20240430091031.png]]