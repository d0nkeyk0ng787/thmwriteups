# More Windows Forensics

### Event Logs

To easily find the total **TurnedOnTime** the system was on for, Nirsoft has a tool named **TurnedOnTimesView** which will give you a nice output. Launch the tool via NirLaunched, then load the system.evtx like so: Options -> Advanced Options -> Data Sources: External Disk, then browse to the location of your Windows event logs and load them. Hit OK and it will give you what you need.

### User Activity

To view the most recently logged on user, Nirsoft has a tool named **WinLogOnView** which can be launched via NirLauncher. Then simply load the System.evtx like so: Options -> Advanced Options -> Data Sources: External Disk, then browse to the location of your Windows event logs and load them. Hit OK and your user logon info will be output.

### MFT

A guide to parsing the MFT can be found on [my github page](https://github.com/d0nkeyk0ng787/thmwriteups/blob/main/DFIR/Windows-Forensics/Windows-Forensics-2/CTF%20Writeups/THM/DFIR/Windows%20Forensics%202%20-%20THM%20Room.md), from there, open the CSV file in excel or Eric Zimmermans EZViewer. This is a great place to find information such as Created, LastModified, LastRecordedChange, LastAccess, LogFileSequenceNumber, etc.

### SRUM Database

The System Resources Usage Monitor is a built-in feature in modern Windows operating systems that collects statistics related to the execution of binaries. From here information can be found such as the bytes sent and received by an application, the UserId, SID, Username, etc. This information can be extracted from the **SRUDB.dat** file found in the **C:\Windows\System32\sru\SRUDB.dat** directory. Eric Zimmerman has a tool named **SrumECmd.exe** which can take the **SRUDB.dat** file and output that information in an easy to analyse csv file. Its usage is as follows:
```posh
SrumECmd.exe -f C:\Windows\System32\sru\SRUDB.dat --csv C:\Users\Administrator\Documents\Srumoutput\srumoutput.csv
```
