# Hamlet - THM Room
### Completed 1120 25 JAN 21
### d0nkeyk0ng787/gnome787

### IP - 10.10.57.233

### Creds

WebAnno: ghost:vnsanctified | admin: | ophelia:1234
FTP: ophelia:KEQehFDWwuQbMbKW

### Enum

Starting with an nmap scan and a gobuster scan

nmap -sC -sV 10.10.98.56 -oN nmap/initial
gobuster dir -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -u http://10.10.98.56 -o gobuster.out -x php,sh,txt,cgi,html,js,css,py

From taking a look at the site we can see there is a user 'Michael ghost Canterbury'

The system seems to have an ftp service with anonymous login enabled. I tried to access the ftp using the terminal but for some reason it wouldn't process any of my commands so I just opended it in the browser. From here I was able to grab two files from the site

We find out that passwords must fall between 12 and 14 characters and be lowercase

I decided to check if there is a robots.txt page and there is. It also happens to contain a flag: THM{1_most_mechanical_and_dirty_hand}

Our nmap scan returned a number of open ports to I am going to have a quick look at those to see if there is anything of interest

There is a login page on port '8080'

We know from the index page that there is a user 'Michael 'ghost' Canterbury', from this I am going to assume that 'ghost' is a username

We can use 'cewl' to generate a wordlist based on the /hamlet.txt file. Using the following command will generate a wordlist we can use with burp that matches our password requirements

Of note here is that the port 8000 has this hamlet.txt file embeded in it and within the source code it has the path to this file, this may be helpful later on when looking for a shell
/repository/project/0/document/0/source/hamlet.txt

cewl -d 2 -m 12 -w words.txt http://10.10.161.53/hamlet.txt --lowercase

Using burpsuite we bruteforce the login and get the password 'vnsanctified'

This works and we can login to the site

We can take a look around and see there are some other users, also a portal for us to upload stuff that currently only contains the hamlet.txt

It stands to reason then, that we could upload our shell and it would in the same directory as the one we found on that embedded page

I quickly had a look at the other user accounts by changing their password and found a password in the annotations for ophelia. This may be ftp or ssh so it's worth a look

These credentials work for FTP, which we see has a flag file and this is flag 3

We can have a look around as there are a few more directories. In the gravediggers directory there is a python script which contains flag 2

Back to our reverse shell, we upload it and try access it at /repository/project/0/document/0/source/php-reverse-shell.php

Ok I had to make a small change to this address to trigger it and thought I would keep this in the writeup. If we take a look at the link, we see that it references project 0 & document 0. Given we used the same project as the hamlet.txt file the project value is fine, however, since it is a different document, we have to change /document/0 to /document/1
Therefore, our link looks like so: /repository/project/0/document/1/source/php-reverse-shell.php

And we have a shell. Unfortunately there is no python to upgrade our shell, we do however find that there is a SUID bit for 'cat' which we can use to read files

Using this we can read the /etc/shadow file which contains the root users password hash: $y$j9T$.9s2wZRY3hcP/udKIFher1$sIBIYsiMmFlXhKOO4ZDJDXo54byuq7a4xAD0k9jw2m4

A bit of searching and we find out that this is a yescrypt hash. We can use either johntheripper or hashcat to crack this

We crack the hash and the root password is 'murder'

If we head to the root directory we see a flag, although it requires the command "ls -la" to show it

I also noticed a directory named 'Stage', when I looked inside it also had a flag
 
Looking around it seemed that this system was not the same one the webanno site is hosted on. Using the command 'cat /proc/1/cgroup' I was able to see that it is a docker instance. We could also simply do 'ls -la' in the '/' directory and see the .dockerenv file

Now we need to escape it

This was an interesting learning curve for me as I hadn't done this before

I started by using the command 'ls -la /dev | grep disk' which checked for any drives on the system

It found number of drives, interesting to us are the 'xvda' drives which on a normal system would be 'sda' drives

I tried mounting the xvda and xvda1 but couldn't. I then tried xvda2 and that worked. For this I used the following commands
mkdir /mnt/xvda2_copy
Then I mounted it
mount /dev/xvda2 /mnt/xvda2_copy

This wasn't exactly what I was looking for

After some research, I tried mount /dev/dm-0 to the same folder
mount /dev/dm-0 /mnt/xvda2_copy

and this gave me the '/' directory of the system hosting the docker container

From there I was able to get the final flag



### Flags

Michaels Password: vnsanctified
Flag 1: THM{1_most_mechanical_and_dirty_hand}
Flag 2: THM{2_ophelia_s_grave}
Flag 3: THM{3_i_was_the_more_deceived}
Flag 4: THM{4_the_murder_of_gonzago}
Flag 5: THM{5_murder_most_foul}
Flag 6: THM{6_though_this_be_madness_yet_there_is_method_in_t}
