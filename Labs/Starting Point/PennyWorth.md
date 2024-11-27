**TCP Scan Results**
```
PORT     STATE SERVICE
8080/tcp open  http-proxy
```

**SYN Scan Results**
```
PORT     STATE SERVICE
8080/tcp open  http-proxy
```


**Ack Scan Results**
```
All 65535 scanned ports on 10.129.33.123 are in ignored states.
```

**Script & Version Scan Results**
```
8080/tcp open  http    Jetty 9.4.39.v20210325
|_http-title: Site doesn't have a title (text/html;charset=utf-8).
|_http-server-header: Jetty(9.4.39.v20210325)
| http-robots.txt: 1 disallowed entry 
|_/
```

**Auth Scan Results**
```
PORT     STATE SERVICE
8080/tcp open  http-proxy
```

**Vuln Scan Results**
```
PORT     STATE SERVICE
8080/tcp open  http-proxy
| http-enum: 
|_  /robots.txt: Robots file
```
