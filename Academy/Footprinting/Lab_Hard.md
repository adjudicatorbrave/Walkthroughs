
# Footprinting Lab - Hard

## Writeup

* Ran a NMAP scan on the box and found the following ports open:
  * 22 - ssh
    * SSH-2.0-OpenSSH_8.2p1 Ubuntu-4ubuntu0
    * Authentication methods supported are publickey and passwords.
  * 143 and 993 - Dovecot imapd
  * 118 amd 995 - Dovecot pop3
  * 62 - DHCP
  * 161 - net-snmp SNMPv3 server
    * `onesixtyone -c ~/HTB/Tools/SecLists/Discovery/SNMP/snmp-onesixtyone.txt 10.129.71.172`
    * `10.129.71.172 [backup] Linux NIXHARD 5.4.0-90-generic #101-Ubuntu SMP Fri Oct 15 20:00:55 UTC 2021 x86_64`
    * `snmpwalk -v2c -c backup 10.129.71.172 > snmpwalk_output`
    * Captured username and password `tom NMds732Js2761"` from SNMP walk
* User/Pass did not work directly in SSH as it requires a SSH key.
* Used user/pass to login to IMAP server and pull emails
    * `1 LOGIN username password 	User's login.`
    * `1 LIST "" *`
    * `1 SELECT INBOX`
    * `1 FETCH 1 all`
    * `1 FETCH 1 BODY[]`
    * The last query is how you see the actual body of the email.
    * The body of the email contains a ssh private key
    * I copied that file into a id_rsa file and modified it's permissions to read only for the user and no other permissions.
    * I then ssh'ed into the server using tom and the ssh key.
    * Once in the system I snooped around in the bash_history file which showed there is a mysql server running.
    * I logged into sql server using tom and his known password.
    * I then looked at the databases on the server and the tables in those DB's.
    * I found a user DB with a user table which contained the user HTB and it's password.
    * Challenge solved.
    * 
* Used captured username and password to gain access 
   * `ssh -v tom@inlanefreight.htb -o PreferredAuthentications=password`

  
* Host Iis Unbuntu and name is NIXHARD


## NMAP Scan Results

### TCP Scan Results
```
PORT    STATE SERVICE
22/tcp  open  ssh
110/tcp open  pop3
143/tcp open  imap
993/tcp open  imaps
995/tcp open  pop3s
```

### SYN Scan Results
```
PORT    STATE SERVICE
22/tcp  open  ssh
110/tcp open  pop3
143/tcp open  imap
993/tcp open  imaps
995/tcp open  pop3s
```

### Version Scan Results
```
PORT    STATE SERVICE  VERSION
22/tcp  open  ssh      OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 3f:4c:8f:10:f1:ae:be:cd:31:24:7c:a1:4e:ab:84:6d (RSA)
|   256 7b:30:37:67:50:b9:ad:91:c0:8f:f7:02:78:3b:7c:02 (ECDSA)
|_  256 88:9e:0e:07:fe:ca:d0:5c:60:ab:cf:10:99:cd:6c:a7 (ED25519)
110/tcp open  pop3     Dovecot pop3d
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=NIXHARD
| Subject Alternative Name: DNS:NIXHARD
| Not valid before: 2021-11-10T01:30:25
|_Not valid after:  2031-11-08T01:30:25
|_pop3-capabilities: PIPELINING AUTH-RESP-CODE TOP UIDL CAPA SASL(PLAIN) RESP-CODES STLS USER
143/tcp open  imap     Dovecot imapd (Ubuntu)
|_imap-capabilities: capabilities ENABLE Pre-login ID LITERAL+ more IDLE SASL-IR post-login listed IMAP4rev1 OK STARTTLS AUTH=PLAINA0001 have LOGIN-REFERRALS
| ssl-cert: Subject: commonName=NIXHARD
| Subject Alternative Name: DNS:NIXHARD
| Not valid before: 2021-11-10T01:30:25
|_Not valid after:  2031-11-08T01:30:25
|_ssl-date: TLS randomness does not represent time
993/tcp open  ssl/imap Dovecot imapd (Ubuntu)
|_imap-capabilities: capabilities ENABLE Pre-login ID LITERAL+ AUTH=PLAINA0001 more IDLE post-login IMAP4rev1 listed SASL-IR OK have LOGIN-REFERRALS
| ssl-cert: Subject: commonName=NIXHARD
| Subject Alternative Name: DNS:NIXHARD
| Not valid before: 2021-11-10T01:30:25
|_Not valid after:  2031-11-08T01:30:25
|_ssl-date: TLS randomness does not represent time
995/tcp open  ssl/pop3 Dovecot pop3d
|_pop3-capabilities: SASL(PLAIN) USER UIDL CAPA AUTH-RESP-CODE PIPELINING TOP RESP-CODES
| ssl-cert: Subject: commonName=NIXHARD
| Subject Alternative Name: DNS:NIXHARD
| Not valid before: 2021-11-10T01:30:25
|_Not valid after:  2031-11-08T01:30:25
|_ssl-date: TLS randomness does not represent time
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

### Vuln Scan Results
```
PORT    STATE SERVICE
22/tcp  open  ssh
110/tcp open  pop3
143/tcp open  imap
993/tcp open  imaps
995/tcp open  pop3s
```

### Auth Scan Results
```
PORT    STATE SERVICE
22/tcp  open  ssh
| ssh-publickey-acceptance: 
|_  Accepted Public Keys: No public keys accepted
| ssh-auth-methods: 
|   Supported authentication methods: 
|     publickey
|_    password
110/tcp open  pop3
143/tcp open  imap
993/tcp open  imaps
995/tcp open  pop3s
```
### UDP Scan Results
```
PORT    STATE         SERVICE
68/udp  open|filtered dhcpc
161/udp open          snmp
```

### UDP SNMP Version Scan Results
```
PORT    STATE SERVICE VERSION
161/udp open  snmp    net-snmp; net-snmp SNMPv3 server
| snmp-info: 
|   enterprise: net-snmp
|   engineIDFormat: unknown
|   engineIDData: 5b99e75a10288b6100000000
|   snmpEngineBoots: 11
|_  snmpEngineTime: 1h45m28s
```

## SSH Audit Results
```
[0;36m# general[0m
[0;32m(gen) banner: SSH-2.0-OpenSSH_8.2p1 Ubuntu-4ubuntu0.3[0m
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
[0;33m(nfo) Potentially insufficient connection throttling detected, resulting in possible vulnerability to the DHEat DoS attack (CVE-2002-20001).  38 connections were created in 0.810 seconds, or 46.9 conns/sec; server must respond with a rate less than 20.0 conns/sec per IPv4/IPv6 source address to be considered safe.  For rate-throttling options, please see <https://www.ssh-audit.com/hardening_guides.html>.  Be aware that using 'PerSourceMaxStartups 1' properly protects the server from this attack, but will cause this test to yield a false positive.  Suppress this test and message with the --skip-rate-test option.[0m
```

## SNMP Walk Output
```
iso.3.6.1.2.1.1.1.0 = STRING: "Linux NIXHARD 5.4.0-90-generic #101-Ubuntu SMP Fri Oct 15 20:00:55 UTC 2021 x86_64"
iso.3.6.1.2.1.1.2.0 = OID: iso.3.6.1.4.1.8072.3.2.10
iso.3.6.1.2.1.1.3.0 = Timeticks: (717729) 1:59:37.29
iso.3.6.1.2.1.1.4.0 = STRING: "Admin <tech@inlanefreight.htb>"
iso.3.6.1.2.1.1.5.0 = STRING: "NIXHARD"
iso.3.6.1.2.1.1.6.0 = STRING: "Inlanefreight"
iso.3.6.1.2.1.1.7.0 = INTEGER: 72
iso.3.6.1.2.1.1.8.0 = Timeticks: (9) 0:00:00.09
iso.3.6.1.2.1.1.9.1.2.1 = OID: iso.3.6.1.6.3.10.3.1.1
iso.3.6.1.2.1.1.9.1.2.2 = OID: iso.3.6.1.6.3.11.3.1.1
iso.3.6.1.2.1.1.9.1.2.3 = OID: iso.3.6.1.6.3.15.2.1.1
iso.3.6.1.2.1.1.9.1.2.4 = OID: iso.3.6.1.6.3.1
iso.3.6.1.2.1.1.9.1.2.5 = OID: iso.3.6.1.6.3.16.2.2.1
iso.3.6.1.2.1.1.9.1.2.6 = OID: iso.3.6.1.2.1.49
iso.3.6.1.2.1.1.9.1.2.7 = OID: iso.3.6.1.2.1.4
iso.3.6.1.2.1.1.9.1.2.8 = OID: iso.3.6.1.2.1.50
iso.3.6.1.2.1.1.9.1.2.9 = OID: iso.3.6.1.6.3.13.3.1.3
iso.3.6.1.2.1.1.9.1.2.10 = OID: iso.3.6.1.2.1.92
iso.3.6.1.2.1.1.9.1.3.1 = STRING: "The SNMP Management Architecture MIB."
iso.3.6.1.2.1.1.9.1.3.2 = STRING: "The MIB for Message Processing and Dispatching."
iso.3.6.1.2.1.1.9.1.3.3 = STRING: "The management information definitions for the SNMP User-based Security Model."
iso.3.6.1.2.1.1.9.1.3.4 = STRING: "The MIB module for SNMPv2 entities"
iso.3.6.1.2.1.1.9.1.3.5 = STRING: "View-based Access Control Model for SNMP."
iso.3.6.1.2.1.1.9.1.3.6 = STRING: "The MIB module for managing TCP implementations"
iso.3.6.1.2.1.1.9.1.3.7 = STRING: "The MIB module for managing IP and ICMP implementations"
iso.3.6.1.2.1.1.9.1.3.8 = STRING: "The MIB module for managing UDP implementations"
iso.3.6.1.2.1.1.9.1.3.9 = STRING: "The MIB modules for managing SNMP Notification, plus filtering."
iso.3.6.1.2.1.1.9.1.3.10 = STRING: "The MIB module for logging SNMP Notifications."
iso.3.6.1.2.1.1.9.1.4.1 = Timeticks: (9) 0:00:00.09
iso.3.6.1.2.1.1.9.1.4.2 = Timeticks: (9) 0:00:00.09
iso.3.6.1.2.1.1.9.1.4.3 = Timeticks: (9) 0:00:00.09
iso.3.6.1.2.1.1.9.1.4.4 = Timeticks: (9) 0:00:00.09
iso.3.6.1.2.1.1.9.1.4.5 = Timeticks: (9) 0:00:00.09
iso.3.6.1.2.1.1.9.1.4.6 = Timeticks: (9) 0:00:00.09
iso.3.6.1.2.1.1.9.1.4.7 = Timeticks: (9) 0:00:00.09
iso.3.6.1.2.1.1.9.1.4.8 = Timeticks: (9) 0:00:00.09
iso.3.6.1.2.1.1.9.1.4.9 = Timeticks: (9) 0:00:00.09
iso.3.6.1.2.1.1.9.1.4.10 = Timeticks: (9) 0:00:00.09
iso.3.6.1.2.1.25.1.1.0 = Timeticks: (718484) 1:59:44.84
iso.3.6.1.2.1.25.1.2.0 = Hex-STRING: 07 E8 0A 0C 03 3A 1F 00 2B 00 00 
iso.3.6.1.2.1.25.1.3.0 = INTEGER: 393216
iso.3.6.1.2.1.25.1.4.0 = STRING: "BOOT_IMAGE=/vmlinuz-5.4.0-90-generic root=/dev/mapper/ubuntu--vg-ubuntu--lv ro ipv6.disable=1 maybe-ubiquity
"
iso.3.6.1.2.1.25.1.5.0 = Gauge32: 0
iso.3.6.1.2.1.25.1.6.0 = Gauge32: 142
iso.3.6.1.2.1.25.1.7.0 = INTEGER: 0
iso.3.6.1.2.1.25.1.7.1.1.0 = INTEGER: 1
iso.3.6.1.2.1.25.1.7.1.2.1.2.6.66.65.67.75.85.80 = STRING: "/opt/tom-recovery.sh"
iso.3.6.1.2.1.25.1.7.1.2.1.3.6.66.65.67.75.85.80 = STRING: "tom NMds732Js2761"
iso.3.6.1.2.1.25.1.7.1.2.1.4.6.66.65.67.75.85.80 = ""
iso.3.6.1.2.1.25.1.7.1.2.1.5.6.66.65.67.75.85.80 = INTEGER: 5
iso.3.6.1.2.1.25.1.7.1.2.1.6.6.66.65.67.75.85.80 = INTEGER: 1
iso.3.6.1.2.1.25.1.7.1.2.1.7.6.66.65.67.75.85.80 = INTEGER: 1
iso.3.6.1.2.1.25.1.7.1.2.1.20.6.66.65.67.75.85.80 = INTEGER: 4
iso.3.6.1.2.1.25.1.7.1.2.1.21.6.66.65.67.75.85.80 = INTEGER: 1
iso.3.6.1.2.1.25.1.7.1.3.1.1.6.66.65.67.75.85.80 = STRING: "chpasswd: (user tom) pam_chauthtok() failed, error:"
iso.3.6.1.2.1.25.1.7.1.3.1.2.6.66.65.67.75.85.80 = STRING: "chpasswd: (user tom) pam_chauthtok() failed, error:
Authentication token manipulation error
chpasswd: (line 1, user tom) password not changed
Changing password for tom."
iso.3.6.1.2.1.25.1.7.1.3.1.3.6.66.65.67.75.85.80 = INTEGER: 4
iso.3.6.1.2.1.25.1.7.1.3.1.4.6.66.65.67.75.85.80 = INTEGER: 1
iso.3.6.1.2.1.25.1.7.1.4.1.2.6.66.65.67.75.85.80.1 = STRING: "chpasswd: (user tom) pam_chauthtok() failed, error:"
iso.3.6.1.2.1.25.1.7.1.4.1.2.6.66.65.67.75.85.80.2 = STRING: "Authentication token manipulation error"
iso.3.6.1.2.1.25.1.7.1.4.1.2.6.66.65.67.75.85.80.3 = STRING: "chpasswd: (line 1, user tom) password not changed"
iso.3.6.1.2.1.25.1.7.1.4.1.2.6.66.65.67.75.85.80.4 = STRING: "Changing password for tom."
iso.3.6.1.2.1.25.1.7.1.4.1.2.6.66.65.67.75.85.80.4 = No more variables left in this MIB View (It is past the end of the MIB tree)
```
