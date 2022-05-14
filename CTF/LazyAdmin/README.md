# ROOM NAME - THM Room

### D0nkeyk0ng787
### COMPLETED 2003 14 MAY 22

### Box IP - 10.10.165.169

### Creds

manager:Password123
rice:randompass

### Enum

Open Ports - 22, 80

Interesting pages found with gobuster - /content

After running the inital scans, we see in /content the site is running Sweetrice CMS

From here we take a look at searchsploit and see there are a number of potential exploits we can use

One of them, being an arbitrary code execution vulnerability allows us to access an admin panel, we do however needs some creds for this. 

Using another of the vulnerabilities, the baackup disclosure vuln, we can find a hashed password for the account 'manager' and use that to login once we crack the hash

From there we upload a shell through the themes section on the page. Catching it with netcat we now have a shell and now have the user flag.

### Priv Esc

Now that we have user access we can priv esc. Running 'sudo -l' we see there is a backup.pl file we can run as sudo. Whilst we can't write to that file itself we can write to another file which this file reads from. 

From here we simply add a reverse shell to the code of that file, run the backup.pl as sudo and catch that root shell with netcat gaining the root flag.

### Flags

User Flag - THM{63e5bce9271952aad1113b6f1ac28a07}
Root Flag - THM{6637f41d0177b6f37cb20d775124699f}