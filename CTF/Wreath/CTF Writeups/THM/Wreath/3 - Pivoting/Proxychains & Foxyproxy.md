# Proxychains & Foxyproxy

Proxies are useful as they allow us to create an open port port on our own attacking machine which is linked to the compromised server, giving us access to the target network

Proxychains and Foxyproxy are two different tools that enable us to achieve this

### Proxychains

Proxychains is a command line tool which enables us to to achieve our goal of redirecting traffic through our open port into the target network

Whilst useful it is not without it's drawbacks, chiefly that it is very slow and doing something such as an nmap scan whilst using proxychains is frustratingly slow

Proxychains is accessed by using the command proxychains which can be added to another command to route that traffic through **proxychains**. An example would be **proxychains** **nc -lnvp 4444** or **proxychains nmap -sC -sV 10.10.10.10**

Proxychains get their configuration from a master config file which is located at **/etc/proxychains.conf** however, first proxychains will attempt to read a configuration file in the **current working directory** followed by **~/.proxychains** and then the **/etc** directory. This means you can simply copy the **proxychains.conf** file from the **/etc** directory and modify it in the current working directory negating the need to mess with the master file

Adding proxies is as simple as inputting the proxy type ip & port in config file under the section proxylist towards the bottom. Of note is the proxy dns section which can cause issues when doing nmap scans so ensure you comment out the **proxy_dns** line

Note
- You can only conduct TCP scans through proxychains using nmap. Any other scan (UDP, SYN) won't work and neither will ping requests so ensure to add the **-Pn** switch
- Always try to use a static binary first as nmap through proxychains is very slow

### Foxyproxy

Froxyproxy is the ideal tool if you need to use a proxy whilst working in a web browser. Namely this is the tool most often used with burpsuite to avoid having to change the browser proxy settings every time you wish to intercept some traffic in burp

Foxyproxy is very easy to use as it has a very straightforward GUI that makes inputting and activating a new proxy very easy