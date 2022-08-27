# Enumeration

Now that we have taken control of the **.150** host, it's time to move onto the **.100** host which we can assume is our targets personal PC that was mentioned in the brief

As always we start with enumeration. Given we have our evil winrm access, we can use that to scan our **.100** host

There are different ways we could do this, one simple method would be to use a tool that comes with the Empire C2 framework called **Invoke-Portscan**. This is a powershell script that we can use to scan the network

Evil Winrm makes it very easy to get this tool onto our target. By simply adding the switch **-s** followed by the directory path to our script into our command to connect to the system, we can have all the tools from that directory on our compromised system

For us this will be: **evil-winrm -u Administrator -H 37db630168e5f82aafa8461e05c6bbd1 -i 10.200.196.150 -s /usr/share/powershell-empire/empire/server/data/module_source/situational_awareness/network/**

Now that we are in, we need to initialise our script by doing **Invoke-Portscan.ps1**. Once it is initialised, we can then run the command to scan our **.100** host. For our scan we want to scan for the top 50 ports which can be done by adding **-TopPorts 50** to our command. In it's entirety, our command will look like **Invoke-Portscan -Hosts 10.200.196.100 -TopPorts 50**

Running our scan and we get some results. We know the host is alive and that it current has ports 80 & 3389 open. We also know that there are some filtered ports and no closed ports



