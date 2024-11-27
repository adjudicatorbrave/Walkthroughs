# SMB Exercise from Footprinting Module

## NMAP Scan Results

### TCP Scan Results
```
PORT      STATE SERVICE
21/tcp    open  ftp
22/tcp    open  ssh
111/tcp   open  rpcbind
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
2049/tcp  open  nfs
40551/tcp open  unknown
45283/tcp open  unknown
54223/tcp open  unknown
58999/tcp open  unknown
```

### SYN Scan Results
```

139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
2049/tcp  open  nfs
40551/tcp open  unknown
45283/tcp open  unknown
54223/tcp open  unknown
58999/tcp open  unknown
```

### Version Scan Results
```
PORT      STATE  SERVICE     VERSION
21/tcp    open   ftp
| fingerprint-strings: 
|   GenericLines: 
|     220 InFreight FTP v1.1
|     Invalid command: try being more creative
|_    Invalid command: try being more creative
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--   1 ftpuser  ftpuser        39 Nov  8  2021 flag.txt
22/tcp    open   ssh         OpenSSH 8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 3f:4c:8f:10:f1:ae:be:cd:31:24:7c:a1:4e:ab:84:6d (RSA)
|   256 7b:30:37:67:50:b9:ad:91:c0:8f:f7:02:78:3b:7c:02 (ECDSA)
|_  256 88:9e:0e:07:fe:ca:d0:5c:60:ab:cf:10:99:cd:6c:a7 (ED25519)
111/tcp   open   rpcbind     2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100003  3           2049/udp   nfs
|   100003  3           2049/udp6  nfs
|   100003  3,4         2049/tcp   nfs
|   100003  3,4         2049/tcp6  nfs
|   100005  1,2,3      35146/udp6  mountd
|   100005  1,2,3      47361/udp   mountd
|   100005  1,2,3      58999/tcp   mountd
|   100005  1,2,3      60009/tcp6  mountd
|   100021  1,3,4      41665/tcp6  nlockmgr
|   100021  1,3,4      43730/udp   nlockmgr
|   100021  1,3,4      45283/tcp   nlockmgr
|   100021  1,3,4      56916/udp6  nlockmgr
|   100227  3           2049/tcp   nfs_acl
|   100227  3           2049/tcp6  nfs_acl
|   100227  3           2049/udp   nfs_acl
|_  100227  3           2049/udp6  nfs_acl
139/tcp   open   netbios-ssn Samba smbd 4.6.2
445/tcp   open   netbios-ssn Samba smbd 4.6.2
2049/tcp  open   nfs         3-4 (RPC #100003)
40551/tcp open   mountd      1-3 (RPC #100005)
45283/tcp open   nlockmgr    1-4 (RPC #100021)
54233/tcp closed unknown
58999/tcp open   mountd      1-3 (RPC #100005)
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port21-TCP:V=7.94SVN%I=7%D=8/31%Time=66D3CAA3%P=aarch64-unknown-linux-g
SF:nu%r(GenericLines,74,"220\x20InFreight\x20FTP\x20v1\.1\r\n500\x20Invali
SF:d\x20command:\x20try\x20being\x20more\x20creative\r\n500\x20Invalid\x20
SF:command:\x20try\x20being\x20more\x20creative\r\n");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
| smb2-time: 
|   date: 2024-09-01T02:00:36
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
|_nbstat: NetBIOS name: DEVSMB, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
|_clock-skew: 14s
```

## Authentication Scan Results
```
PORT      STATE  SERVICE
21/tcp    open   ftp
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--   1 ftpuser  ftpuser        39 Nov  8  2021 flag.txt
22/tcp    open   ssh
| ssh-publickey-acceptance: 
|_  Accepted Public Keys: No public keys accepted
| ssh-auth-methods: 
|   Supported authentication methods: 
|     publickey
|_    password
111/tcp   open   rpcbind
139/tcp   open   netbios-ssn
445/tcp   open   microsoft-ds
2049/tcp  open   nfs
40551/tcp open   unknown
45283/tcp open   unknown
54233/tcp closed unknown
58999/tcp open   unknown
```

## Vulnerability Scan Results
```
PORT      STATE  SERVICE
21/tcp    open   ftp
22/tcp    open   ssh
111/tcp   open   rpcbind
139/tcp   open   netbios-ssn
445/tcp   open   microsoft-ds
2049/tcp  open   nfs
40551/tcp open   unknown
45283/tcp open   unknown
54233/tcp closed unknown
58999/tcp open   unknown

Host script results:
|_smb-vuln-ms10-061: Could not negotiate a connection:SMB: ERROR: Server returned less data than it was supposed to (one or more fields are missing); aborting [9]
|_samba-vuln-cve-2012-1182: Could not negotiate a connection:SMB: ERROR: Server returned less data than it was supposed to (one or more fields are missing); aborting [9]
|_smb-vuln-ms10-054: false
```
