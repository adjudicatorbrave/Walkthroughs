# Easy Level Lab Exercise from Footprinting Module

## Writeup

Ran port scan on IP.  Here are the open ports:
```
PORT     STATE SERVICE
21/tcp   open  ftp 
22/tcp   open  ssh
53/tcp   open  domain
2121/tcp open  ccproxy-ftp
```

Steps taken:
1. Used provided credentials to log into ccproxy-ftp.
2. With ftp access, I downloaded the ssh pub and private keys and the bash history
3. Bash history showed that the user had created a flag file and moved into a flag directory in /home/
4. Used ssh keys to log into server and get flag file.
5. Note: I did need to modify the permissions of the ssh files


## NMAP Scan Results

### TCP Scan Results
```
PORT     STATE SERVICE
21/tcp   open  ftp 
22/tcp   open  ssh
53/tcp   open  domain
2121/tcp open  ccproxy-ftp
```

### SYN Scan Results
```
PORT     STATE SERVICE
21/tcp   open  ftp
22/tcp   open  ssh
53/tcp   open  domain
2121/tcp open  ccproxy-ftp
```

### Version Scan Results
```
PORT     STATE SERVICE VERSION
21/tcp   open  ftp
| fingerprint-strings: 
|   GenericLines: 
|     220 ProFTPD Server (ftp.int.inlanefreight.htb) [10.129.51.177]
|     Invalid command: try being more creative
|_    Invalid command: try being more creative
22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 3f:4c:8f:10:f1:ae:be:cd:31:24:7c:a1:4e:ab:84:6d (RSA)
|   256 7b:30:37:67:50:b9:ad:91:c0:8f:f7:02:78:3b:7c:02 (ECDSA)
|_  256 88:9e:0e:07:fe:ca:d0:5c:60:ab:cf:10:99:cd:6c:a7 (ED25519)
53/tcp   open  domain  ISC BIND 9.16.1 (Ubuntu Linux)
| dns-nsid: 
|_  bind.version: 9.16.1-Ubuntu
2121/tcp open  ftp
| fingerprint-strings: 
|   GenericLines: 
|     220 ProFTPD Server (Ceil's FTP) [10.129.51.177]
|     Invalid command: try being more creative
|_    Invalid command: try being more creative
2 services unrecognized despite returning data. If you know the service/version, please submit the following fingerprints at https://nmap.org/cgi-bin/submit.cgi?new-service :
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port21-TCP:V=7.94SVN%I=7%D=9/25%Time=66F4CD10%P=aarch64-unknown-linux-g
SF:nu%r(GenericLines,9C,"220\x20ProFTPD\x20Server\x20\(ftp\.int\.inlanefre
SF:ight\.htb\)\x20\[10\.129\.51\.177\]\r\n500\x20Invalid\x20command:\x20tr
SF:y\x20being\x20more\x20creative\r\n500\x20Invalid\x20command:\x20try\x20
SF:being\x20more\x20creative\r\n");
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port2121-TCP:V=7.94SVN%I=7%D=9/25%Time=66F4CD10%P=aarch64-unknown-linux
SF:-gnu%r(GenericLines,8D,"220\x20ProFTPD\x20Server\x20\(Ceil's\x20FTP\)\x
SF:20\[10\.129\.51\.177\]\r\n500\x20Invalid\x20command:\x20try\x20being\x2
SF:0more\x20creative\r\n500\x20Invalid\x20command:\x20try\x20being\x20more
SF:\x20creative\r\n");
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose|WAP|router
Running (JUST GUESSING): Linux 3.X|2.4.X|2.6.X (90%), MikroTik RouterOS 6.X (88%)
OS CPE: cpe:/o:linux:linux_kernel:3.2.0 cpe:/o:linux:linux_kernel:2.4.20 cpe:/o:mikrotik:routeros:6.15 cpe:/o:linux:linux_kernel:2.6.22 cpe:/o:linux:linux_kernel:2.6.18
Aggressive OS guesses: Linux 3.2.0 (90%), Tomato 1.27 - 1.28 (Linux 2.4.20) (90%), MikroTik RouterOS 6.15 (Linux 3.3.5) (88%), Tomato 1.28 (Linux 2.4.20) (88%), Tomato firmware (Linux 2.6.22) (88%), Linux 2.6.18 (86%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

### Vuln Scan Results
```
```

### Auth Scan Results
```
```
