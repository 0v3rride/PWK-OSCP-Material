Linux | BSD | *nix

Enumeration: 
* General
	* Good searches for information and exploits
		* Google
		* Cve databases (exploit-db, security focus, etc)
		* Vulners
		* github
	* Nmap
		* nmap -sV -v -O -p- -T5 --script=vuln <ip>
		* nmap -A -O -T5 -p- -v <ip>
		* nmap -A -O -T5 -v -s<U|S|T> <ip>
	* Reconnoitre
		* reconnoitre -t x.x.x.x --services -o path
	* Reconscan.py
* Http[s]
	* NSE, Nmap
	* Gobuster, dirb, dirbuster
		* Robots.txt
	* Nikto
		* HTTP methods (PUT, DELETE)
		* CGI (shellshock via cgi)
		* Directories
		* Robots.txt
	* Curl
	* Web page source code can reveal the version and name of the web application if neither of that information is displayed explicitly
	* Also various files like readme, version, license, etc. can reveal the version name and other information about the web application
* Smb
	* Rpcclient -U "<username" <IP Address>
	* Smbclient
	* Enum4linux
	* NSE, Nmap
	* MSF modules
	* Nmblookup
	* nbtscan
* Snmp
	* NSE, nmap (udp scan)
		* nmap -A -O -T5 -v -s<U|S|T> <ip>
	* Snmpwalk
		* snmpwalk -c public -v1 x.x.x.x
	* Onesixtyone with seclist community strings list
* Smtp
	* NSE, nmap
	* smtp-user-enum
* Dns
	* NSE, Nmap
	* Dnsrecon
	* Dnsenum
	* Dig - dig <domain>@<target ip>
	* Host command
* Passwords:
	* Make wordlist from webpage
		* cewl -w list.txt -a -e  --with-numbers x.x.x.x
	* Default passwords for default configurations
	* Blank passwords with usernames
	* No password or username
	* Easily guessable passwords
		* hydra -L userlist.txt -P /root/Desktop/lists/wordlists/rockyou.txt ssh://x.x.x.x -V -t 40 -e nsr -o ssh_found
	* Try reusing passwords you get across multiple services enabled on a target as well as users (Alpha)
* Other information that you obtain from other machines you popped could help so be on the look out for information that may lead to a scenario like this (alice and bethany)

Enumeration & Recon:
* Nmap 
* NSE vuln scan
* Nikto
* Dirbuster/gobuster
* Check web directories
* Check web page source code
* Scan TCP and UDP
* Check for default creds

* Ports:

Vuln Identification:

Exploitation:

Post-Exploitation:
Priv esc:
* Pspy tools to catch processes firing off
* User information
	* Whoami 
	* Id
* Get OS and kernel information
	* Lsb_release -a 
	* Uname -a
	* Uname -mrs
* Check environment variables
	* Env
	* Set
* Look for suid or guid bit set on files and software with suid vulns
	* Find / -user root -perm -4000 -type f 2>/dev/null
	* Find / -user root -perm -2000 -type f 2>/dev/null
	* find / -perm -u=s -type f 2>/dev/null
	* find / -perm -g=s -type f 2>/dev/null
	* Nmap
	* Vim
	* Less
	* More
	* Cp
	* Nano
	* Mv
	* Find
* Look for folders and files with certain permissions
	* World writable folders
		□ find / -writable -type d 2>/dev/null
		□ find / -perm -222 -type d 2>/dev/null
		□ find / -perm -o w -type d 2>/dev/null
	* World executable folders
		□ find / -perm -o x -type d 2>/dev/null
	* World writable and executable folder
		find / \( -perm -o w -perm -o x \) -type d 2>/dev/null
* Look in ssh folder
	* /root/.ssh
	* /home/user/.ssh
* Unmounted filesystems
	* mount -l
	* cat /etc/fstab
* NFS Shares
	* Showmount -e <ip>
	* Mount <ip>/directory /attacker/tmp
* Cronjobs/scheduled jobs
	* Crontab -l
	* ls -alh /var/spool/cron
	* ls -al /etc/ | grep cron
	* ls -al /etc/cron*
	* cat /etc/cron*
	* cat /etc/at.allow
	* cat /etc/at.deny
	* cat /etc/cron.allow
	* cat /etc/cron.deny
	* cat /etc/crontab
	* cat /etc/anacrontab
	* cat /var/spool/cron/crontabs/root
* Cat out passwd or shadow
	* /etc/passwd - has priority over shadow (change password in this first)
	* /etc/shadow
* Check history or bash_history files
	* History
	* Cat .bash_history
* List services running
	* Ps aux
	* Ps -ef
	* Top
	* Cat /etc/services
* User installed software
	* /usr/local/
	* /usr/local/src
	* /usr/local/bin
	* /usr/bin
	* /sbin/
	* /var/cache/apt/archives0
	* /var/cache/yum
	* /opt/
	* /home
	* /var/
	* /usr/src/
	* Dpkg -l (debian/ubuntu
	* rpm -qa (CentOS / openSUSE )
	* pkg_info (*BSD)
* Places and things to check for
	* config.php
	* /var/spool/mail
	* grep -i user [filename]
	* grep -i pass [filename]
	* find / -iname "*password*" 2>/dev/null
	* Grep -ri <string> <directory>
	* cat /etc/passwd
	* cat /etc/group
	* cat /etc/shadow
	* ls -alh /var/mail/
	* ls -ahlR /root/
	* ls -ahlR /home/
	* cat /var/apache2/config.inc
	* cat /var/lib/mysql/mysql/user.MYD
	* cat /root/anaconda-ks.cfg
	* cat ~/.bashrc
	* cat ~/.profile
	* cat /var/mail/root
	* cat /var/spool/mail/root
* List networking information
	* netstat -anlp
	* netstat -ano
* World writeable directories for storing exploit code, etc.
	* /tmp
	* /var/tmp
	* /dev/shm
	* /var/spool/vbox
	* /var/spool/samba
