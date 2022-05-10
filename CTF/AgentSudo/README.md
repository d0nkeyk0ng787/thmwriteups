# Agent Sudo - THM Room

## d0nkeyk0ng787

### Box IP - 

### Creds

chris:crystal

### Enum

Open Ports - 21 ftp, 22 ssh, 80 http

From here we take a look at the site and see it contains a messsage. It says to use our own codename as the user agent from agent R. Based on this I changed my user agent to A, B and then C which returned another message to a guy named 'chris'. It suggests chris's password is weak so we might as well try bruteforce it.

Using hydra we are able to get a password for chris. Using this password we can login to the ftp server and continue looking around. In the ftp server there are 2 images and a text file. The text file suggests that a password is inside one of the images.

Using stegseek we find a passphrase "Area51" inside one of the images. We also get another file 'message.txt' which contains the name of Agent J as well as another password 'hackerrules!' which is likely an ssh login. 

Before we ssh into the server its worth taking a look at that other image. Using binwalk, we can see that it contains a zip file inside. From here we can use zip2john and then john to crack the zip and gain access. Gaining access to the zip file there is a message inside that contains an interesting string 'QXJlYTUx'

### Priv Esc

Now that we have user level access we can try and get root access. 

Side note - The incident related to the image that is on the server is the "roswell alien autopsy"

Now onto priv esc. Running the command sudo -l we see that the user can run /bin/bash as root

I tried running 'sudo /bin/bash' but it didn't work so I did a quick google search to find any known sudo vulnerabilities on exploitdb. It came up with 'CVE-2019-14287' which looks like it will work for this case.

Using the payload 'sudo -u#-1 /bin/bash' we can gain root access to the server and gain the root flag as well as agent R's name deskel.

### Flags

User Flag - b03d975e8c92a7c04146cfa7a5a313c7
Root Flag - b53a02f55b57d4439e3341834d70c062
