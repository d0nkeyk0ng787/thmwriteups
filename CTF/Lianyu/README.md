# Lian_Yu - THM Room
### Completed 2315 11 JAN 22
### d0nkeyk0ng787/gnome787

### Box IP - 10.10.187.2

### Creds

FTP Account: vigilante:!#th3h00d
SSH Account: slade:M3tahuman

### Enumeration

We begin with an nmap scan: nmap -sC -sV 10.10.187.2 -oN nmap/initial

We also launch up a gobuster scan: gobuster dir -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -u http://10.10.187.2 -o gobuster.out

This gives us the page '/island'

This site doesn't contain much but the source code does reveal a hidden word 'vigilante'

From here we can try and find another sub directory using gobuster: gobuster dir -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -u http://10.10.187.2/island -o gobuster2.out

This gives us another page '/2100'

This page again doesn't contain much but it does suggest there is a '.ticket' file somewhere on this site

From here we can try and find that with gobuster: gobuster dir -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -u http://10.10.187.2/island/2100 -x .ticket -o gobuster2.out

This gives us a page that contains the following 'RTy8yhBQdscX'

To me this looks like it could be base58 so using cyberchef we can crack it and we get '!#th3h00d'

Now we can try and login to the ftp

This works and we can grab the files on the server

We can use stegseek to extract the ssh password from the 'aa.jpg' file which is 'M3tahuman'

We now need the username. I'm going to try 'slade' as that was another directory on the ftp server

This account works and we are in

User Flag: THM{P30P7E_K33P_53CRET5__C0MPUT3R5_D0N'T}

### Priv Esc

Starting out with the command 'sudo -l' to list all the commands this user can run as sudo

This returns '/usr/bin/pkexec' which has a GTFOBins payload we can use to get a root shell: 'sudo pkexec /bin/sh'

Root Flag: THM{MY_W0RD_I5_MY_B0ND_IF_I_ACC3PT_YOUR_CONTRACT_THEN_IT_WILL_BE_COMPL3TED_OR_I'LL_BE_D34D}
