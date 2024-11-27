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
PORT     STATE SERVICE
21/tcp   open  ftp
22/tcp   open  ssh
53/tcp   open  domain
2121/tcp open  ccproxy-ftp
```

### Auth Scan Results
```
PORT     STATE SERVICE
21/tcp   open  ftp
22/tcp   open  ssh
| ssh-auth-methods: 
|   Supported authentication methods: 
|     publickey
|_    password
| ssh-publickey-acceptance: 
|_  Accepted Public Keys: No public keys accepted
53/tcp   open  domain
2121/tcp open  ccproxy-ftp
```

## Bash History File 

```
ls -al
mkdir ssh
cd ssh/
echo "test" > id_rsa
id
ssh-keygen -t rsa -b 4096
cd ..
rm -rf ssh/
ls -al
cd .ssh/
cat id_rsa
ls a-l
ls -al
cat id_rsa.pub >> authorized_keys
cd ..
cd /home
cd ceil/
ls -l
ls -al
mkdir flag
cd flag/
touch flag.txt
vim flag.txt 
cat flag.txt 
ls -al
mv flag/flag.txt .
```

## SSH Audit Results
```
[0;36m# general[0m
[0;32m(gen) banner: SSH-2.0-OpenSSH_8.2p1 Ubuntu-4ubuntu0.2[0m
[0;32m(gen) software: OpenSSH 8.2p1[0m
[0;32m(gen) compatibility: OpenSSH 7.4+, Dropbear SSH 2020.79+[0m
[0;32m(gen) compression: enabled (zlib@openssh.com)[0m

[0;36m# security[0m
[0;33m(cve) CVE-2021-41617                        -- (CVSSv2: 7.0) privilege escalation via supplemental groups[0m
[0;33m(cve) CVE-2020-15778                        -- (CVSSv2: 7.8) command injection via anomalous argument transfers[0m
[0;33m(cve) CVE-2016-20012                        -- (CVSSv2: 5.3) enumerate usernames via challenge response[0m

[0;36m# key exchange algorithms[0m
[0;32m(kex) curve25519-sha256                     -- [info] available since OpenSSH 7.4, Dropbear SSH 2018.76[0m
[0;32m                                            `- [info] default key exchange from OpenSSH 7.4 to 8.9[0m
[0;32m(kex) curve25519-sha256@libssh.org          -- [info] available since OpenSSH 6.4, Dropbear SSH 2013.62[0m
[0;32m                                            `- [info] default key exchange from OpenSSH 6.5 to 7.3[0m
[0;31m(kex) ecdh-sha2-nistp256                    -- [fail] using elliptic curves that are suspected as being backdoored by the U.S. National Security Agency[0m
                                            `- [info] available since OpenSSH 5.7, Dropbear SSH 2013.62
[0;31m(kex) ecdh-sha2-nistp384                    -- [fail] using elliptic curves that are suspected as being backdoored by the U.S. National Security Agency[0m
                                            `- [info] available since OpenSSH 5.7, Dropbear SSH 2013.62
[0;31m(kex) ecdh-sha2-nistp521                    -- [fail] using elliptic curves that are suspected as being backdoored by the U.S. National Security Agency[0m
                                            `- [info] available since OpenSSH 5.7, Dropbear SSH 2013.62
[0;32m(kex) diffie-hellman-group-exchange-sha256 (3072-bit) -- [info] available since OpenSSH 4.4[0m
[0;32m                                                      `- [info] OpenSSH's GEX fallback mechanism was triggered during testing. Very old SSH clients will still be able to create connections using a 2048-bit modulus, though modern clients will use 3072. This can only be disabled by recompiling the code (see https://github.com/openssh/openssh-portable/blob/V_9_4/dh.c#L477).[0m
[0;32m(kex) diffie-hellman-group16-sha512         -- [info] available since OpenSSH 7.3, Dropbear SSH 2016.73[0m
[0;32m(kex) diffie-hellman-group18-sha512         -- [info] available since OpenSSH 7.3[0m
[0;33m(kex) diffie-hellman-group14-sha256         -- [warn] 2048-bit modulus only provides 112-bits of symmetric strength[0m
                                            `- [info] available since OpenSSH 7.3, Dropbear SSH 2016.73

[0;36m# host-key algorithms[0m
[0;32m(key) rsa-sha2-512 (3072-bit)               -- [info] available since OpenSSH 7.2[0m
[0;32m(key) rsa-sha2-256 (3072-bit)               -- [info] available since OpenSSH 7.2, Dropbear SSH 2020.79[0m
[0;31m(key) ssh-rsa (3072-bit)                    -- [fail] using broken SHA-1 hash algorithm[0m
                                            `- [info] available since OpenSSH 2.5.0, Dropbear SSH 0.28
                                            `- [info] deprecated in OpenSSH 8.8: https://www.openssh.com/txt/release-8.8
[0;31m(key) ecdsa-sha2-nistp256                   -- [fail] using elliptic curves that are suspected as being backdoored by the U.S. National Security Agency[0m
[0;33m                                            `- [warn] using weak random number generator could reveal the key[0m
                                            `- [info] available since OpenSSH 5.7, Dropbear SSH 2013.62
[0;32m(key) ssh-ed25519                           -- [info] available since OpenSSH 6.5, Dropbear SSH 2020.79[0m

[0;36m# encryption algorithms (ciphers)[0m
[0;33m(enc) chacha20-poly1305@openssh.com         -- [warn] vulnerable to the Terrapin attack (CVE-2023-48795), allowing message prefix truncation[0m
                                            `- [info] available since OpenSSH 6.5, Dropbear SSH 2020.79
                                            `- [info] default cipher since OpenSSH 6.9
[0;32m(enc) aes128-ctr                            -- [info] available since OpenSSH 3.7, Dropbear SSH 0.52[0m
[0;32m(enc) aes192-ctr                            -- [info] available since OpenSSH 3.7[0m
[0;32m(enc) aes256-ctr                            -- [info] available since OpenSSH 3.7, Dropbear SSH 0.52[0m
[0;32m(enc) aes128-gcm@openssh.com                -- [info] available since OpenSSH 6.2[0m
[0;32m(enc) aes256-gcm@openssh.com                -- [info] available since OpenSSH 6.2[0m

[0;36m# message authentication code algorithms[0m
[0;33m(mac) umac-64-etm@openssh.com               -- [warn] using small 64-bit tag size[0m
                                            `- [info] available since OpenSSH 6.2
[0;32m(mac) umac-128-etm@openssh.com              -- [info] available since OpenSSH 6.2[0m
[0;32m(mac) hmac-sha2-256-etm@openssh.com         -- [info] available since OpenSSH 6.2[0m
[0;32m(mac) hmac-sha2-512-etm@openssh.com         -- [info] available since OpenSSH 6.2[0m
[0;31m(mac) hmac-sha1-etm@openssh.com             -- [fail] using broken SHA-1 hash algorithm[0m
                                            `- [info] available since OpenSSH 6.2
[0;33m(mac) umac-64@openssh.com                   -- [warn] using encrypt-and-MAC mode[0m
[0;33m                                            `- [warn] using small 64-bit tag size[0m
                                            `- [info] available since OpenSSH 4.7
[0;33m(mac) umac-128@openssh.com                  -- [warn] using encrypt-and-MAC mode[0m
                                            `- [info] available since OpenSSH 6.2
[0;33m(mac) hmac-sha2-256                         -- [warn] using encrypt-and-MAC mode[0m
                                            `- [info] available since OpenSSH 5.9, Dropbear SSH 2013.56
[0;33m(mac) hmac-sha2-512                         -- [warn] using encrypt-and-MAC mode[0m
                                            `- [info] available since OpenSSH 5.9, Dropbear SSH 2013.56
[0;31m(mac) hmac-sha1                             -- [fail] using broken SHA-1 hash algorithm[0m
[0;33m                                            `- [warn] using encrypt-and-MAC mode[0m
                                            `- [info] available since OpenSSH 2.1.0, Dropbear SSH 0.28

[0;36m# fingerprints[0m
[0;32m(fin) ssh-ed25519: SHA256:AtNYHXCA7dVpi58LB+uuPe9xvc2lJwA6y7q82kZoBNM[0m
[0;32m(fin) ssh-rsa: SHA256:gYf8woxUQ0oVmCnny4K4fmsnijf3aZcdeZGb2kKxPUQ[0m

[0;36m# algorithm recommendations (for OpenSSH 8.2)[0m
[0;31m(rec) -ecdh-sha2-nistp256                   -- kex algorithm to remove [0m
[0;31m(rec) -ecdh-sha2-nistp384                   -- kex algorithm to remove [0m
[0;31m(rec) -ecdh-sha2-nistp521                   -- kex algorithm to remove [0m
[0;31m(rec) -ecdsa-sha2-nistp256                  -- key algorithm to remove [0m
[0;31m(rec) -hmac-sha1                            -- mac algorithm to remove [0m
[0;31m(rec) -hmac-sha1-etm@openssh.com            -- mac algorithm to remove [0m
[0;31m(rec) -ssh-rsa                              -- key algorithm to remove [0m
[0;33m(rec) -chacha20-poly1305@openssh.com        -- enc algorithm to remove [0m
[0;33m(rec) -diffie-hellman-group14-sha256        -- kex algorithm to remove [0m
[0;33m(rec) -hmac-sha2-256                        -- mac algorithm to remove [0m
[0;33m(rec) -hmac-sha2-512                        -- mac algorithm to remove [0m
[0;33m(rec) -umac-128@openssh.com                 -- mac algorithm to remove [0m
[0;33m(rec) -umac-64-etm@openssh.com              -- mac algorithm to remove [0m
[0;33m(rec) -umac-64@openssh.com                  -- mac algorithm to remove [0m

[0;36m# additional info[0m
[0;33m(nfo) For hardening guides on common OSes, please see: <https://www.ssh-audit.com/hardening_guides.html>[0m
[0;33m(nfo) Potentially insufficient connection throttling detected, resulting in possible vulnerability to the DHEat DoS attack (CVE-2002-20001).  38 connections were created in 0.741 seconds, or 51.3 conns/sec; server must respond with a rate less than 20.0 conns/sec per IPv4/IPv6 source address to be considered safe.  For rate-throttling options, please see <https://www.ssh-audit.com/hardening_guides.html>.  Be aware that using 'PerSourceMaxStartups 1' properly protects the server from this attack, but will cause this test to yield a false positive.  Suppress this test and message with the --skip-rate-test option.[0m
```
