# SMTP Exercise from Footprinting Module

## NMAP Scan Results

### TCP Scan Results

```
PORT      STATE SERVICE
22/tcp    open  ssh
25/tcp    open  smtp
53/tcp    open  domain
110/tcp   open  pop3
143/tcp   open  imap
993/tcp   open  imaps
995/tcp   open  pop3s
3306/tcp  open  mysql
33060/tcp open  mysqlx
```

### SYN Scan Results
```
PORT      STATE SERVICE
22/tcp    open  ssh
25/tcp    open  smtp
53/tcp    open  domain
110/tcp   open  pop3
143/tcp   open  imap
993/tcp   open  imaps
995/tcp   open  pop3s
3306/tcp  open  mysql
33060/tcp open  mysqlx
```
### SMTP Port Scan - Version Detection
```
PORT   STATE SERVICE VERSION
25/tcp open  smtp    Postfix smtpd
|_smtp-commands: mail1, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN, SMTPUTF8, CHUNKING
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 5.0 (99%), Linux 4.15 - 5.8 (95%), Linux 5.0 - 5.4 (95%), Linux 5.3 - 5.4 (95%), Linux 2.6.32 (95%), Linux 5.0 - 5.5 (95%), Linux 3.1 (94%), Linux 3.2 (94%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), HP P2000 G3 NAS device (93%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: Host: InFreight

```

### SMTP Port Scan - Authentication Scan

```
PORT   STATE SERVICE
25/tcp open  smtp
| smtp-enum-users: 
|   root
|   admin
|   administrator
|   webadmin
|   sysadmin
|   netadmin
|   guest
|   user
|   web
|_  test
```

### SMTP Port Scan - Vulns

```
PORT   STATE SERVICE
25/tcp open  smtp
| smtp-vuln-cve2010-4344: 
|_  The SMTP server is not Exim: NOT VULNERABLE
```


### SMTP Port Scan - Open Relay Scan
```
PORT   STATE SERVICE
25/tcp open  smtp
| smtp-open-relay: Server is an open relay (16/16 tests)
|  MAIL FROM:<> -> RCPT TO:<relaytest@nmap.scanme.org>
|  MAIL FROM:<antispam@nmap.scanme.org> -> RCPT TO:<relaytest@nmap.scanme.org>
|  MAIL FROM:<antispam@InFreight> -> RCPT TO:<relaytest@nmap.scanme.org>
|  MAIL FROM:<antispam@[10.129.132.174]> -> RCPT TO:<relaytest@nmap.scanme.org>
|  MAIL FROM:<antispam@[10.129.132.174]> -> RCPT TO:<relaytest%nmap.scanme.org@[10.129.132.174]>
|  MAIL FROM:<antispam@[10.129.132.174]> -> RCPT TO:<relaytest%nmap.scanme.org@InFreight>
|  MAIL FROM:<antispam@[10.129.132.174]> -> RCPT TO:<"relaytest@nmap.scanme.org">
|  MAIL FROM:<antispam@[10.129.132.174]> -> RCPT TO:<"relaytest%nmap.scanme.org">
|  MAIL FROM:<antispam@[10.129.132.174]> -> RCPT TO:<relaytest@nmap.scanme.org@[10.129.132.174]>
|  MAIL FROM:<antispam@[10.129.132.174]> -> RCPT TO:<"relaytest@nmap.scanme.org"@[10.129.132.174]>
|  MAIL FROM:<antispam@[10.129.132.174]> -> RCPT TO:<relaytest@nmap.scanme.org@InFreight>
|  MAIL FROM:<antispam@[10.129.132.174]> -> RCPT TO:<@[10.129.132.174]:relaytest@nmap.scanme.org>
|  MAIL FROM:<antispam@[10.129.132.174]> -> RCPT TO:<@InFreight:relaytest@nmap.scanme.org>
|  MAIL FROM:<antispam@[10.129.132.174]> -> RCPT TO:<nmap.scanme.org!relaytest>
|  MAIL FROM:<antispam@[10.129.132.174]> -> RCPT TO:<nmap.scanme.org!relaytest@[10.129.132.174]>
|_ MAIL FROM:<antispam@[10.129.132.174]> -> RCPT TO:<nmap.scanme.org!relaytest@InFreight>

```


