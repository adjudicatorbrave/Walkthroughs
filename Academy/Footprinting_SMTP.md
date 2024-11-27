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
### SMTP Port Scan - Vulns

```

PORT   STATE SERVICE
25/tcp open  smtp
| smtp-vuln-cve2010-4344: 
|_  The SMTP server is not Exim: NOT VULNERABLE
```
