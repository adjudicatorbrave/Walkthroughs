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
```

### SYN Scan Results
```
```

### Version Scan Results
```
```

### Vuln Scan Results
```
```

### Auth Scan Results
```
```
