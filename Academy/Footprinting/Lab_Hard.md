
# Footprinting Lab - Hard

## Writeup

* Ran a NMAP scan on the box and found the following ports open:
  * 22 - ssh
    * SSH-2.0-OpenSSH_8.2p1 Ubuntu-4ubuntu0
    * Authentication methods supported are publickey and passwords.
  * 143 and 993 - Dovecot imapd
  * 118 amd 995 - Dovecot pop3
  * 62 - DHCP
  * 161 - net-snmp SNMPv3 server
    * `onesixtyone -c ~/HTB/Tools/SecLists/Discovery/SNMP/snmp-onesixtyone.txt 10.129.71.172`
    * `10.129.71.172 [backup] Linux NIXHARD 5.4.0-90-generic #101-Ubuntu SMP Fri Oct 15 20:00:55 UTC 2021 x86_64`
    * `snmpwalk -v2c -c backup 10.129.71.172 > snmpwalk_output`
    * Captured username and password `tom NMds732Js2761"` from SNMP walk
* User/Pass did not work directly in SSH as it requires a SSH key.
* Used user/pass to login to IMAP server and pull emails
    * `1 LOGIN username password 	User's login.`
    * `1 LIST "" *`
    * `1 SELECT INBOX`
    * `1 FETCH 1 all`
    * `1 FETCH 1 BODY[]`
    * The last query is how you see the actual body of the email.
    * The body of the email contains a ssh private key
    * I copied that file into a id_rsa file and modified it's permissions to read only for the user and no other permissions.
    * I then ssh'ed into the server using tom and the ssh key.
    * Once in the system I snooped around in the bash_history file which showed there is a mysql server running.
    * I logged into sql server using tom and his known password.
    * I then looked at the databases on the server and the tables in those DB's.
    * I found a user DB with a user table which contained the user HTB and it's password.
    * Challenge solved.
    * 
* Used captured username and password to gain access 
   * `ssh -v tom@inlanefreight.htb -o PreferredAuthentications=password`

  
* Host Iis Unbuntu and name is NIXHARD
