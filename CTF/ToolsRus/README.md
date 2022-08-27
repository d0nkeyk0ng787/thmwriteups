# ToolsRus - THM Room

#### Gnome787 | 27 AUG 22

### Creds

bob:bubbles

### Enumeration

Starting with enumeration we run a gobuster scan ```gobuster dir -w /usr/share/seclists/Discovery/Web-Content/common.txt -u http://10.10.158.186``` and an nmap scan ```nmap -sC -sV 10.10.158.186```

![gobusterscan](/Screenshots/gobuster.png)
![nmapscan](/Screenshots/nmapscan1.png)

Our gobuster scan gives us the answer for our first three questions. We have some directories and from the **guidelines** directory a username **bob** and a potential attack vector, an insecure **Tomcat** server.

First, it's worth running a bruteforce with hyrda to see if we can gain access to that **/protected** directory. The command for that is:
```hydra -l bob -P /usr/share/wordlists/rockyou.txt -t 1 -f 10.10.158.186 http-get /protected/```

From there we get a result: **bubbles**
![hydra](/Screenshots/hydra.png)

We see this doesn't really get us anywhere, what a shame.

Our nmape scan returns some useful information:
1. 22 - ssh
2. 80 - http
3. 1234 - Tomcat
4. 8009 - Jserv

It would be worth running another gobuster scan, this time on the port **1234** to see what is hosted on that tomcat site.
```gobuster dir -w /usr/share/seclists/Discovery/Web-Content/common.txt -u http://10.10.158.186:1234```

![gobusterscan2](/Screenshots/gobuster2.png)

We see there are a number of directories, the **manager** directory can be accessed with those credetials we got earlier. THM tells us to run a **nikto** scan on the **/manager/html** directory. 

```nikto -id bob:bubbles -h http://10.10.158.186:1234/manager/html```

This scan is likely going to take a while so whilst I waited I had a look for a metasploit exploit for Apache/2.4.18. Metasploit has one we can use **exploit/multi/http/tomcat_mgr_upload** which will use the information we have already gathered to gain a shell. 

We launch up the exploit an gain a meterpreter shell we can use to get the root flag. 

We can also run the command ```shell``` followed by ```python -c 'import pty;pty.spawn ("/bin/bash")'``` to get ourselves a normal system shell with a prompt.

### Priv Esc


### Questions

1. Guidelines
2. bob
3. Protected
4. bubbles
5. 1234
6. Apache Tomcat/7.0.88
7. 5
8. Apache/2.4.18
9. 1.1
10. root
11. ff1fc4a81affcc7688cf89ae7dc6e0e1
