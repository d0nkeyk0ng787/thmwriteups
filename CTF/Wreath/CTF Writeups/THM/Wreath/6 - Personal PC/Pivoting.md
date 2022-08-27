# Pivoting

Before we can begin exploiting this system, we need to be able to access it. Therefore we are going to have to do some pivoting

For this we are going to use chisel. This will work well with our current sshuttle connection. Before we can do that, we need to add a rule to the firewall to allow our chosen port to open a connection through it. This can be done by typing the following into our evil-winrm console: **netsh advfirewall firewall add rule name="Chisel-gnome787" dir=in action=allow protocol=tcp localport=30500
**

Now that we have our firewall rule in place, we need to grab a Windows binary for chisel which can be found here [Chisel Binaries](https://github.com/jpillora/chisel/releases). From there we can use evil-winrm's inbuild **upload** features to get that binary onto our compromised system. This is done by placing the file in the folder we are currently in, in that terminal and then using the command **upload chisel-gnome787.exe** which will upload that file onto the system

From there we need to start the chisel server which we can do by using the following command: **.\chisel-gnome787.exe server -p 30500 --socks5**, making sure to set the port as the one we just opened up in the firewall

We now want to start the chisel client on our attacking machine (making sure to do **sudo apt install chisel** if you don't already have it installed). From there we want to connect our client to the server by using the following command **chisel client 10.200.196.150:30500 8888:socks**, making sure to use the port you set the server to use. The local listening port can be whatever you choose and making sure to set it as a socks

Once the connection is setup, we can head to our browser and setup the proxy in foxyproxy. For this we want to configure it like so: **IP:127.0.0.1 PORT:8888 TYPE: SOCKS5**, making sure it is sets as **SOCKS5** as by default it is HTTP

From there we should be able to access the site and using the Wappalyzer plugin see that it is using PHP 7.4.11
