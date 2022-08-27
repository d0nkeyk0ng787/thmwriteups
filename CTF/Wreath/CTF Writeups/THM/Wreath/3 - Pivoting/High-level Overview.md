# High-level Overview

Like everything when it comes to pentesting, the target system dictactes how we do things. Pivoting is no different, what method we use to pivot is dependent on the system and network we are compromising and trying to pivot through

There are two main methods for pivoting, they are:
- **Tunnelling/Proxying:** This involves creating a proxy like connection through the compromised machine which will allow us to route all desired traffic into the targeted network. This method is good beause it can potentially be tunnelled inside another protocol which would aid in evading IDS detection
- **Port Forwarding:** As simple as creating a connection between a local port and a single port on the target machine via a compromised host

**Benefits**
- **Proxy:** Tends to be good for redirecting lots of different kinds of traffic into our targeted network. E.G an nmap scan or accessing multiple different ports on multiple different machines
- **Port Forwarding:** Faster & more reliable but limited to a single or small range of port(s)

At this point of a pentest you would begin drawing up a network layout (already done for us) and conduct further enumeration to determine the best method of pivoting
