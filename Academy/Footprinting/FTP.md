
# FTP Exercise from Footprinting Module

## NMAP Scan Results

### TCP Scan Results
```
No results?
```

### SYN Scan Results
```
No results?
```

### Version Scan Results
```
PORT   STATE SERVICE VERSION
21/tcp open  ftp     ProFTPD
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 3f:4c:8f:10:f1:ae:be:cd:31:24:7c:a1:4e:ab:84:6d (RSA)
|   256 7b:30:37:67:50:b9:ad:91:c0:8f:f7:02:78:3b:7c:02 (ECDSA)
|_  256 88:9e:0e:07:fe:ca:d0:5c:60:ab:cf:10:99:cd:6c:a7 (ED25519)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

### Vuln Scan Results
```
PORT   STATE SERVICE
21/tcp open  ftp
22/tcp open  ssh
```

### Auth Scan Results
```
PORT   STATE SERVICE
21/tcp open  ftp
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--   1 ftpuser  ftpuser        39 Nov  8  2021 flag.txt
22/tcp open  ssh
| ssh-auth-methods: 
|   Supported authentication methods: 
|     publickey
|_    password
| ssh-publickey-acceptance: 
|_  Accepted Public Keys: No public keys accepted
```

