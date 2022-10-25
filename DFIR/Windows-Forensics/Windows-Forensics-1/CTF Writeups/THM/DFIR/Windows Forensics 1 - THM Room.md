# Windows Forensics 1 - THM Room

### Windows Registry and Forensics

**Windows Registry** is a database that contains a range of system configuration data including hardware data, software data and user information as well as information about recently used files, programs used, and devices connected to the system. 

A built-in Windows tool named **regedit.exe** can be used to view this database. The registry consits of **Keys** and **Values** and each folder in regedit are registry keys that have corresponding registry values. **Registry Hives** are a group of keys, subkeys and values stored in a single file on the disk.

##### Registry Structure
*   HKEY_CURRENT_USER
*   HKEY_HKEY_USERS
*   HKEY_LOCAL_MACHINE
*   HKEY_CLASSES_ROOT
*   HKEY_CURRENT_CONFIG

### Accessing Registry Hives Offline

In the case you only have a disk image of a system, you would have to navigate the the files for each registry hive. The 5 major hives are located at **C:\Windows\System32\Config**. 

There are 2 other hives that contain user information which can be found on the system. **NTUSER.dat** which is located at **C:\Users\username\** and **USRCLASS.dat** which is located at **C:\Users\username\AppData\Local\Microsoft\Windows**. NTUSER.DAT is mounted on HKCU when the user logs in and USRCLASS.DAT is mounted on HKCU\Software\CLASSES.

##### AmCache Hive

AmCache hive is a file that contains information on files that were recently run on the system. It can be located at **C:\Windows\AppCompat\Programs\Amcache.hve**.

##### Transaction Logs and Backups

**Transaction Logs** contain useful information about a registry hive including changes that haven't yet been written to the registry yet. Each hive has it's own transaction log file stored in the **C:\Windows\System32\Config** directory with the extension **.LOG**.

**Registry backups** are backups of the registry hives located at C:\Windows\System32\Config. These backups are copied to the directory **C:\Windows\System32\Config\RegBack** every 10 days. 

### Data Acquistion

When performing registry forensics, it is best practice to make a copy of the registry and perform your analysis on that copy. There are a range of tools that can be used for this, these include:
* KAPE - is a live data acquistion and analysis tool, primarily a command line tool, however, it does have a GUI version.
* Autopsy - can be used to acquire data from an image or a live system. 
* FTK Imager - same functions as Autopsy. FTK Imager also has an **Obtain Protected Files** options which can be used on a live system, allowing you to extract all the registry hives. It won't extract the Amcache hive unfortunately.

### Exploring Windows Registry

The following are tools used to view the registry data once you have extracted it:
* Registry Viewer - Similar interface to regedit, can only do 1 hive at a time and can't include the transaction logs.
* Zimmermans Registry Explorer - Can load multiple hives simultaneously and add data from transaction logs into the hive allowing for a cleaner and more up-to-date hive. Also has a bookmarks features that lets you jump straight to interesting registry keys and values.
* RegRipper - Takes a registry hive as input and outputs a report containing information about forensically important keys and values from that hive. RegRipper can't take transaction logs so you must first use Registry Explorer to add the transaction logs and then put that hive through RegRipper.

### System Information and System Accounts

Now that we have our registry data, we need to start looking through it to perform a forensic analysis.

##### OS Version

We can find the OS version at ```SOFTWARE\Microsoft\Windows NT\CurrentVersion```

##### Current Control Set

Control Sets are the machines configuration data used for controlling startup within the hive. There are typically two control sets ```ControlSet001``` and ```ControlSet002```, the former being the control set the computer booted with and the latter being the last known good configuration. These can be found at ```SYSTEM\ControlSet001``` and ```SYSTEM\ControlSet002```. 

We can view the CurentControlSet by finding ```SYSTEM\Select\Current``` and the LastKnownGood control set by finding ```SYSTEM\Select\LastKnownGood```.

##### Computer Name

The computer name is stored at:
* ```SYSTEM\CurrentControlSet\Control\ComputerName\ComputerName```.

##### Time Zone Information

Time zone can be found at:
* ```SYSTEM\CurrentControlSet\Control\TimeZoneInformation```

##### Network Interfaces and Past Networks

Network interfaces can be found at:
* ```SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces```

Past networks can be found in the following locations:
* ```SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Signatures\Unmanaged``` 
* ```SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Signatures\Managed```.

##### Autostart Programs (Autorun)

The following keys contain information relating to programs and commands that run when a user logs in:
* ```NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Run```
* ```NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\RunOnce```
* ```SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce```
* ```SOFTWARE\Microsoft\Windows\CurrentVersion\policies\Explorer\Run```
* ```SOFTWARE\Microsoft\Windows\CurrentVersion\Run```

Information about services can be found at:
* ```SYSTEM\CurrentControlSet\Services```

##### SAM Hive and user information

The SAM hive contains various user account, login, and group information. It is stored at:
* ```SAM\Domains\Account\Users```

### Usage or Knowledge of files/folders

##### Recent Files

Windows stores a list of recently used files at:
* ```NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs```
There is also seperate keys within this hive for the most recently used files for different file extensions including **.pdf**, **.jpg**, and **.docx**. For example, the most recently used .pdf files can be found at:
* ```NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs\.pdf```

##### Office Recent Files

Microsoft Office has its own list of recently opened documents at:
* ```NTUSER.DAT\Software\Microsoft\Office\VERSION```
For example, Word 15 would look like:
* ```NTUSER.DAT\Software\Microsoft\Office\15.0\Word```

##### ShellBags

ShellBags store user configurations and preferences for folders and the desktop. These can be found at:
* ```USRCLASS.DAT\Local Settings\Software\Microsoft\Windows\Shell\Bags```
* ```USRCLASS.DAT\Local Settings\Software\Microsoft\Windows\Shell\BagMRU```
* ```NTUSER.DAT\Software\Microsoft\Windows\Shell\BagMRU```
* ```NTUSER.DAT\Software\Microsoft\Windows\Shell\Bags```

##### Open/Save & LastVisited Dialog MRUs

When we save a file, Windows prompts us to enter a location. This location is then remembered by the OS which means we can find the registry key(s) of where this information is stored:
* ```NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\OpenSavePIDlMRU```
* ```NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\LastVisitedPidlMRU```

##### Windows Explorer/Search Bars

The paths typed in Windows Explorer search bar are stored in the registry at:
* ```NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\TypedPaths```
* ```NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\WordWheelQuery```

### Evidence of Execution

##### UserAssist

The User Assist registry keys keep track of applications launched using Windows Explorer. Applications launched via the CLI are not tracked here. These keys can be found at:
* ```NTUSER.DAT\Software\Microsoft\Windows\Currentversion\Explorer\UserAssist\{GUID}\Count```

##### ShimCache

ShimCache (also known as AppCompatCache) keeps track of application compatability with the OS as well as tracks all applications launched on the machine. It can be found at:
* ```SYSTEM\CurrentControlSet\Control\Session Manager\AppCompatCache```
**NOTE:** It's worth noting that Registry Explorer isn't able to pass ShimCache data. Therefore, we must use a tool called **AppCompatCache Parser** by Eric Zimmerman. It takes the SYSTEM hive as input, parses the data, and outputs the data as a CSV file which can be viewed in **EZ Viewer**. Usage looks something like:
* ```AppCompatCacheParser.exe --csv <path to save output> -f <path to SYSTEM hive for data parsing> -c <control set to parse>```

##### AmCache

AmCache is an artifact related to ShimCache that stores additional information related to program executions including SHA1 hashes of the executed program. The AmCache registry hive can be found at:
* ```C:\Windows\appcompat\Programs\Amcache.hve```
Information about the lasted executed programs can be found at:
* ```Amcache.hve\Root\File\{Volume GUID}\```

##### BAM/DAM

Background Activity Monitor (BAM) keeps track of background applications whilst Desktop Activity Monitor (DAM) optimises the power consumption of the device. Both these functions are part of the **Modern Standby** system in Windows. Information about BAM and DAM can be found at:
* ```SYSTEM\CurrentControlSet\Services\bam\UserSettings\{SID}```
* ```SYSTEM\CurrentControlSet\Services\dam\UserSettings\{SID}```

### External Devices/USB Device Forensics

The following locations contain information about any USB or removable devices that have been attached to the system at some stage.

##### Device Identification

The following registry keys store information about USBs plugged into the system:
* ```SYSTEM\CurrentControlSet\Enum\USBSTOR```
* ```SYSTEM\CurrentControlSet\Enum\USB```

##### First/Last Times

The following registry key keeps track of when a device was first connected, last time it was connected, and the last time the device was removed from the system:
* ```SYSTEM\CurrentControlSet\Enum\USBSTOR\Ven_Prod_Version\USBSerial#\Properties\{83da6326-97a6-4088-9453-a19231573b29}\####```
This key contains **####** which can be replaced by 1 of the following:
* 0064 - First Connection Time
* 0066 - Last Connection Time
* 0067 - Last Removal Time

##### USB Device Volume Name

Device name of connected drives can be found here:
* ```SOFTWARE\Microsoft\Windows Portable Devices\Devices```