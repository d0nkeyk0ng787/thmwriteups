# Empire: Overview

### Basic Usage & Setup

Now that we have Empire installed, we can go over how to acces it and some basic usage

Before we can do anything we need to launch an Empire server on our attacking machine. The command we use for that is: **sudo powershell-empire server**

**Note**: In a real world pentest, it would be more likely that there would be a dedicated C2 server to host the Empire server module that would allow pentesters to remote into and do what they need to do

Now that we have a server, we can connect to it via the Empire client. This is launched using the command: **powershell-empire client**

Obviously this is connected to our server hosted on the local host, however, if your server is not on the local host there are a couple ways to connect to it. Firstly, you could add the connection information to the config file that is located at **/usr/share/powershell-empire/empire/client/config.yaml**, otherwise you could simply specify in the empire CLI the following command **connect HOSTNAME --username=USERNAME --password=PASSWORD** obviously entering the necessary information for your desired system

Now that we have an Empire server running, we can start Starkiller by executing the command **starkiller** in our attacking machines terminal

Starkiller connects to a REST API that is exposed by launching the Empire server. By default we can connect to it on **127.0.0.1:1337** with the following credentials: **empireadmin:password123**

### Overview

Powershell Empire is made up of a few key modules. These are:
 - **Listeners:** Essentially listen for a connection and aid with exploitation
 - **Stagers:** Payloads created by Empire which enable for a robust reverse shell that works in conjunction with a listener. They are also the delivery mechanism for agents
 - **Agents:** Essentially agents are the connection to the target, the same as a metasploit session they are a connection to the target that allow for further interaction with said target
 - **Modules:** Aid in further exploitation along with agents

One awesome feature of Empire is the ability to add in custom plugins which adds extra functionality to the framework

Starkiller also contains a credential storage feature that will automatically store any credentials found in a local database

As we know from our pentest thus far, our gitserver doesn't have the ability to connect straight back to our attacking machine. Due to this, we will need to setup a special kind of listener so that we can connect to our gitserver target using our C2 framework