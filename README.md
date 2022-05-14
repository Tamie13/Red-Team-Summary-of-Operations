# Red Team: Summary of Operations

## Table of Contents

-  Exposed Services
-  Critical Vulnerabilities
-  Exploitation

### Network Scan To Identify All Available Networks:

-  nmap -sS -PO 192.168.1.*

<img src="https://github.com/Tamie13/Red-Team-Summary-of-Operations/blob/main/Attack%20Target%201%20Images/nmap%20-sS%20-PO%20192.168.1.*.png" width="800" height="800">


### Target Identified
   
   -  Name:  **Target 1**
   -  IP Address: 192.168.1.110

#### Target Machine Scan

-  Nmap scan results for each machine reveal the below services and OS details:
    -  nmap -sV 192.168.110

<img src="https://github.com/Tamie13/Red-Team-Summary-of-Operations/blob/main/Attack%20Target%201%20Images/nmap%20-sV%20192.168.1.110.png" width="600" height="600">

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
    -  3.  User password hash unsalted
    -  4.  Able to escalate user privilege to root

As can be seen below the scan was successful in enumerating valid usernames of **Steven & Michael**

(Click On Image/s To Open In Expanded View)

<img src="https://github.com/Tamie13/Red-Team-Summary-of-Operations/blob/main/Attack%20Target%201%20Images/wpscan%20enumeration%201.png" width="400" height="400"> <img src="https://github.com/Tamie13/Red-Team-Summary-of-Operations/blob/main/Attack%20Target%201%20Images/wpscan%20enumeration_2.png" width="400" height="400">

## Exploitatoin of Vulnerabilities

-  Wordpress user enumeration: 
   - **Command Used** WordPress User Enumeration Scan:  wpscan --url http://192.168.1.110/wordpress --enumerate u

  -  **Vulnerability** CVE-2017-5487:  Scan enumerates user names and other possibly vulnerable paths and files.

  -  **Rating** Base Score: 5.3 Medium

-  Weak user password:
  -  Weak user passwords made it possible to guess and SSH into the target machine with **Username - Michael**.
     -  ssh michael@192.168.1.110
        -  PW = michael
  -  After login as Michael was able to 'cd' into directories run 'ls' and found **Flag One** in the following directory and file:
     -  /var/www/html/service.html
     -  cat service.html
 
  <img src="https://github.com/Tamie13/Red-Team-Summary-of-Operations/blob/main/Attack%20Target%201%20Images/flag-1.png" width="900" height="400">
  

### Exploitation

TODO: Fill out the details below. Include screenshots where possible.


-  The Red Team was able to penetrate Target 1 and retrieve the following confidential data:

#### Target 1


flag1.txt: TODO: Insert flag1.txt hash value


### Exploit Used

TODO: Identify the exploit used


TODO: Include the command run





flag2.txt: TODO: Insert flag2.txt hash value


### Exploit Used

TODO: Identify the exploit used

TODO: Include the command run# Red-Team-Summary-of-Operations

