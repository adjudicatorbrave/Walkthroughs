# MySQL Exercise from Footprinting Module

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
PORT     STATE SERVICE VERSION
3306/tcp open  mysql   MySQL 8.0.27-0ubuntu0.20.04.1
| mysql-info: 
|   Protocol: 10
|   Version: 8.0.27-0ubuntu0.20.04.1
|   Thread ID: 10
|   Capabilities flags: 65535
|   Some Capabilities: Support41Auth, InteractiveClient, SwitchToSSLAfterHandshake, FoundRows, LongColumnFlag, Speaks41ProtocolOld, LongPassword, SupportsLoadDataLocal, Speaks41ProtocolNew, SupportsTransactions, SupportsCompression, DontAllowDatabaseTableColumn, ODBCClient, IgnoreSpaceBeforeParenthesis, IgnoreSigpipes, ConnectWithDatabase, SupportsAuthPlugins, SupportsMultipleStatments, SupportsMultipleResults
|   Status: Autocommit
|   Salt: 2pD\x169	\x04*e;!Y\x13\x01Mc8)\x13 
|_  Auth Plugin Name: caching_sha2_password
```

### Vuln Scan Results
```
PORT     STATE SERVICE
3306/tcp open  mysql
```

### Auth Scan Results
```
PORT     STATE SERVICE
3306/tcp open  mysql
```
