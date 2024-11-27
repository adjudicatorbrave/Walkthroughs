
# FTP Exercise from Footprinting Module

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

### Version Scan Results
```
PORT    STATE SERVICE  VERSION
110/tcp open  pop3     Dovecot pop3d
|_pop3-capabilities: PIPELINING UIDL SASL AUTH-RESP-CODE CAPA TOP STLS RESP-CODES
| ssl-cert: Subject: commonName=dev.inlanefreight.htb/organizationName=InlaneFreight Ltd/stateOrProvinceName=London/countryName=UK
| Not valid before: 2021-11-08T23:10:05
|_Not valid after:  2295-08-23T23:10:05
|_ssl-date: TLS randomness does not represent time
143/tcp open  imap     Dovecot imapd
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=dev.inlanefreight.htb/organizationName=InlaneFreight Ltd/stateOrProvinceName=London/countryName=UK
| Not valid before: 2021-11-08T23:10:05
|_Not valid after:  2295-08-23T23:10:05
|_imap-capabilities: LOGINDISABLEDA0001 more STARTTLS post-login LITERAL+ listed OK ENABLE IDLE ID capabilities have SASL-IR LOGIN-REFERRALS IMAP4rev1 Pre-login
993/tcp open  ssl/imap Dovecot imapd
| ssl-cert: Subject: commonName=dev.inlanefreight.htb/organizationName=InlaneFreight Ltd/stateOrProvinceName=London/countryName=UK
| Not valid before: 2021-11-08T23:10:05
|_Not valid after:  2295-08-23T23:10:05
|_imap-capabilities: more have post-login LITERAL+ listed OK ENABLE IDLE ID capabilities AUTH=PLAINA0001 SASL-IR IMAP4rev1 LOGIN-REFERRALS Pre-login
|_ssl-date: TLS randomness does not represent time
995/tcp open  ssl/pop3 Dovecot pop3d
|_pop3-capabilities: PIPELINING UIDL USER AUTH-RESP-CODE CAPA TOP SASL(PLAIN) RESP-CODES
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=dev.inlanefreight.htb/organizationName=InlaneFreight Ltd/stateOrProvinceName=London/countryName=UK
| Not valid before: 2021-11-08T23:10:05
|_Not valid after:  2295-08-23T23:10:05
```

### Vuln Scan Results
```
PORT    STATE SERVICE
110/tcp open  pop3
143/tcp open  imap
993/tcp open  imaps
995/tcp open  pop3s
```

### Auth Scan Results
```
PORT    STATE SERVICE
110/tcp open  pop3
143/tcp open  imap
993/tcp open  imaps
995/tcp open  pop3s
```

