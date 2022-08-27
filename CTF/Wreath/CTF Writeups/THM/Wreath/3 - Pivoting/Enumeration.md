# Enumeration

We begin pivoting the same as everything else in pentesting, by enumerating. We need to gain an understanding of our target. The more we know about our target, the more options we have available to us. 

There are 5 possible ways to enumerate a network through a compromised host:
- Using material found on the machine. The hosts file or ARP cache, for example
- Using pre-installed tools
- Using statically compiled tools
- Using scripting techniques
- Using local tools through a proxy

This enumeration list is written in order of most desirable to least. Using local tools through a proxy is very slow and therefore is not ideal if it can be avoided. Ideally we would use already installed apps on the machine such as nmap if its on the system. Alternatively we could transfer a static binary or create a script that would perform the scan we need

We can check the arp cache by typing the command **arp -a**. This will give us the ip addresses of any hosts the target has interacted with recently

We can also find static mappings in **/etc/hosts** for Linux and **C:\Windows\System32\drivers\etc\hosts** for Windows. Sometimes you can find misconfigured local DNS servers on linux in the **/etc/resolv.conf file**. This file can be read with **nmcli dev show**. **ipconfig /all** is the easiest way to check the DNS servers on a Windows system

One possible solution to not having any pre-installed tools on the system to scan the network is using a static copy of a tool. A static binary as opposed to a dynamic binary is a program that has been compiled with all its dependencies inside of the finished program. Most programs use external libraries which are on the system. For Windows these are .dll files and for linux they are .so files.

Another option that a pentester may utilise would be a living off the land approach that involves using an installed shell to perform a sweep of the network. If you had a network172.16.0.x/24, the following bash one-liner would perform a full ping sweep of that network

```bash
for i in {1..255}; do (ping -c 1 172.16.0.${i} | grep "bytes from" &); done
```


