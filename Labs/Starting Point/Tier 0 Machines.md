## Meow - telnet
    - Used telnet to connect to machine.
    - Username was root and there was no password
    - telnet {IP}
    - once I had telnet access w/ root, I executed the following scripts:
        - ls -la which showed flag file.
        - I used cat to print the contents of the flag file

## Fawn - ftp

    - Used ftp to connect to server
    - ftp server alllowed anonymous access.
    - Anonymous access was called out as being open by nmap
    - ftp {ip}
    - once I had ftp access I used ls command to list out files which showed flag
    - I used the get command to download the flag to my local machine.

## Dancing - SMB vuln

    - Used nmap -a {ip} to discover open ports.
    - nmap showed smb was running open on the server.
    - used smbclient -L {IP} to get a list shares which showed
        - Admin
        - C$
        - IPC$
        - WorkShares
    - I used smbclient \\\\{IP}\\WorkShares to connect to target, when prompted for password I submitted a blank password which was accepted.
    - Exploring the directories, I found the flag.txt file in James.P and got a copy using get command.

## Redeemer

    - Initial nmap scan did not show any open ports
    - I used nmap -p- {IP} to discover port 6379 open
    - I used nmap -p 6379 -sC {IP} which only showed 6379 open with Redis
    - I used nmap -p 6379 -sV {IP} which showed redis is running version 5.0.7
    - ExploitDB and SearchSploiit showed no open vulns to exploit.
    - I tried to simply connect to redis using redis-cli
    - redis-cli -h {IP} -p {PORT} to connect to server which worked.
    - redis db is not secured with any sort of user name and password which meant i was able to connect and get access to the system.
    - Once I connected, I used KEYS * to get a list of stores(?) which returned flag as one of the items.
    - I then used GET flag to get the flag.

## Explosion

    - Initial nmap scan showed 3389 port open
    - i used nmap -sC {IP} to get more detailed information
    - Used xfreerdp /u:Administrator /p: /v:{IP} to gain desktop access
    - flag file was shown on the desktop.

## PreIgnition

    - Initial nmap scan showed port 80 open.
    - used nmap -sV and whatweb + curl -IL to determine that it’s nginx 1.14.2 used to host site.
    - Used gobuster to discover /admin.php page
    - Searched on the web for default password to nginx 1.14.2 and found that it was admin/admin
    - Logged into the site using default creds.

## Mongod

    - Initial nmap scan showed SSH and MongoDB as externally visibile.
    - Used nmap -sV to determine that it’s MongoDB 3.6.8
    - I used searchsploit to search for vulns on this version of MongoDB and there were none.
    - I connected to the db using mongosh —host {ip} —port {port} which successfully connected
    - Used show dbs command to get list of dbs. One of the db’s was called sensitive_information and I figured that was interesting.
    - Got list of collections using show collections which showed flag collection.
    - used db.flag.find() to get flag.

## Synced

    - This guide was useful on rsync hacking https://www.netspi.com/blog/technical/network-penetration-testing/linux-hacking-case-studies-part-1-rsync/
    - Initial nmap scan showed port 837 open running rync
    - version scanning with nmap showed it’s rsync protocal 31
    - Commands to get to flag file:
        - rsync {ip}:: — this gave me a collections of folders available.  Public was shown as a folder
        - rsync {ip}::public gave me a list of files which showed flag file.
        - I then did rsync {ip}::public/flag.txt .  to get the flag file and submit it.
