Windows

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
* Dump lsass, lsa and cache (mimikatz, fgdump, pwdump)
* Systeminfo 
	* Get windows version
	* Get build number
	* Google for exploits <windows version and build, SP exploit>
* User information
	* Get current user's privileges, group memberships, etc
	* Whoami /priv, /groups
	* Net user <current username>
	* Qwinsta - other users logged in
	* Echo %username% | %userdomain%
	* hostname
* List installed programs
	* Tasklist /SVC
	* dir /a "C:\Program Files"
	* dir /a "C:\Program Files (x86)"
	* reg query HKEY_LOCAL_MACHINE\SOFTWARE
* List and check services for weak permissions
	* Sc query
	* Sc qc <service name>
	* Net start
	* Accesschk.exe -ucqv|-uwcqv <service name>
* Net commands:
	* Net users <user>
	* Net localgroup
	* Net accounts
	* Net start
* List networking information
	* Ipconfig /all
	* Route print
	* Arp -a
	* Netstat -ano
	* Netsh dump
* Check for cached credentials
	* Cmdkey /list (can use runas /savecred)
	* Wce (windows credential editor)
	* dir C:\Users\username\AppData\Local\Microsoft\Credentials\
	* dir C:\Users\username\AppData\Roaming\Microsoft\Credentials\
* Check for weak file permissions
	* Icacls.exe
	* Cacls.exe
	* Full permissions for everyone or users
		* icacls "C:\Program Files\*" 2>nul | findstr "(F)" | findstr "Everyone"
		* icacls "C:\Program Files (x86)\*" 2>nul | findstr "(F)" | findstr "Everyone"
		* icacls "C:\Program Files\*" 2>nul | findstr "(F)" | findstr "BUILTIN\Users"
		* icacls "C:\Program Files (x86)\*" 2>nul | findstr "(F)" | findstr "BUILTIN\Users"
	* Modify permissions for everyone or user
		* icacls "C:\Program Files\*" 2>nul | findstr "(M)" | findstr "Everyone"
		* icacls "C:\Program Files (x86)\*" 2>nul | findstr "(M)" | findstr "Everyone"
		* icacls "C:\Program Files\*" 2>nul | findstr "(M)" | findstr "BUILTIN\Users" 
		* icacls "C:\Program Files (x86)\*" 2>nul | findstr "(M)" | findstr "BUILTIN\Users"
	* Accesschk.exe - writable files
		* accesschk.exe -qwsu "Everyone" *
		* accesschk.exe -qwsu "Authenticated Users" *
		* accesschk.exe -qwsu "Users" *
	* Accesschk.exe - writeable folders
		* accesschk.exe -uwdqs Users c:\
		* accesschk.exe -uwdqs "Authenticated Users" c:\
	* Accesschk.exe - reconfigurations on services
		* accesschk.exe -uwcqv "Everyone" *
		* accesschk.exe -uwcqv "Authenticated Users" *
		* accesschk.exe -uwcqv "Users" *
* Unquoted service paths
	* wmic service get name,displayname,pathname,startmode 2>nul |findstr /i "Auto" 2>nul |findstr /i /v "C:\Windows\\" 2>nul |findstr /i /v """
* List drivers
	* Driverquery
* Check firewall state 
	* Netsh firewall show state
	* Netsh firewall show config
* List scheduled tasks
	* Schtasks /query /fo LIST /v
	* At <hour>:<min> "exe" - schedule task privilege escalation xp,2k3
* WMIC
	* Wmic qfe - list os/kernel
	* wmic qfe get Caption,Description,HotFixID,InstalledOn
	* wmic qfe get Caption,Description,HotFixID,InstalledOn | findstr /C:"KB.." /C:"KB.."
* Check following directories for credentials
	* c:\sysprep.inf
	* c:\sysprep\sysprep.xml
	* %WINDIR%\Panther\Unattend\Unattended.xml
	* %WINDIR%\Panther\Unattended.xml
	* Services\Services.xml: Element-Specific Attributes
	* ScheduledTasks\ScheduledTasks.xml: Task Inner Element, TaskV2 Inner Element, ImmediateTaskV2 Inner Element
	* Printers\Printers.xml: SharedPrinter Element
	* Drives\Drives.xml: Element-Specific Attributes
	* DataSources\DataSources.xml: Element-Specific Attributes
	* %SYSTEMROOT%\repair\SAM
	* %SYSTEMROOT%\System32\config\RegBack\SAM
	* %SYSTEMROOT%\System32\config\SAM
	* %SYSTEMROOT%\repair\system
	* %SYSTEMROOT%\System32\config\SYSTEM
	* %SYSTEMROOT%\System32\config\RegBack\system
* Group Policy Preference saved passwords
* Search for file name with the following or certain file extensions
	* dir /s *pass* == *cred* == *vnc* == *.config*
	* findstr /si password *.xml *.ini *.txt
	* dir /s/b/a | findstr "proof.txt"
* Searching the registry
	* AlwaysInstallElevated reg kek
		* reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer\AlwaysInstallElevated
		* reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer\AlwaysInstallElevated
	* Search registry for passwords
		* reg query HKLM /f password /t REG_SZ /s
		* reg query HKCU /f password /t REG_SZ /s
	* Autologon credentials
		* reg query "HKLM\SOFTWARE\Microsoft\Windows NT\Currentversion\Winlogon" 2>nul | findstr "DefaultUserName DefaultDomainName DefaultPassword"
* Misc
	*  echo %path%
	* accesschk.exe -dqv "C:\Python27"
