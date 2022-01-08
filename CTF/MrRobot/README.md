# Mr Robot CTF - THM Room
### COMPLETED 1517 08 JAN 22
### d0nkeyk0ng787/gnome787

### Creds

wp-login: elliot:ER28-0652
user account: robot:abcdefghijklmnopqrstuvwxyz

### Flag 1

Simple enumeration of the site shows that 'robots.txt' contains an entry which reads 'key-1-of-3.txt'

Going to this file on the site gives us the first key

Flag 1: '073403c8a58a1f80d943455fb30724b9'

### Flag 2

We also have a dictionary file in the 'robots.txt' file which we will download as it might be useful later

After running a gobuster search, we get a number of web pages. Of note is the wordpress login form

We can use 'wpscan' to brute force the login, using the 'fsociety.dic' file we already downloaded

Whilst we could use this dictionary as is, it would take a substantial amount of time given the word count

Instead we can sort the list with the following command, removing duplicate words
'sort fsocity.dic| uniq > sorted.dic'

We also want to try and find a username before we start the brute forcing as it will take substantially longer to brute force if we are using 
a dictionary for both username and password inputs

Interestingly, when we attempt to login to the site, we get the error 'invalid username'. From here we can try some different combos and find that
when we use the username 'elliot' we get a different error saying the password is incorrect for this user.

Now we can startup our wpscan and try and bruteforce the login
'wpscan --url http://10.10.184.201/wp-login.php -U elliot -P sorted.dic'

This returns a password of 'ER28-0652'

From past experience I know that you can get a reverse shell by creating a custom theme and putting the php code in there so that is what I will attempt first

We simply modify the '404.php' file by pasting in the 'php-reverse-shell' code, startup a nc listener and capture the shell

First I will upgrade the shell to make it more stable and usable

Navigating around we find the 2nd key but can't access it so instead find the following
'robot:c3fcd3d76192e4007dfb496cca67e13b'

We will use john the ripper to crack this, which returns
'abcdefghijklmnopqrstuvwxyz'

We can use this to login to the user 'robot' and get the 2nd key

Flag 2: 822c73956184f694993bede3eb39f959

### Flag 3

Now we have to priv esc

We try 'sudo -l' to no avail

Next I try 'find / -type f -perm -04000 -ls 2>/dev/null'. Interestingly we see nmap in there, after searching GTFOBins I find an exploit for it which allows us to 
write to a file

For this we can write to the /etc/passwd file a user with root access

So after trying this I couldn't seem to get it to work. Not sure if it was just user error regardless, I did some research and found that you can use the interactive mode
of nmap to execute shell commands. So by doing
'nmap --interactive'
and then
'!sh'

I was able to set the id of my robot account to root

From there I was able to get the final flag

Flag 3: 04787ddef27c3dee1ee161b21670b4e4
