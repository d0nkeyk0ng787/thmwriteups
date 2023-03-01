# Windows Forensics 1 - THM Room

### The FAT File Systems

FAT or **File Allocation Table** is the original default file system for Windows. FAT creates a table that indexes the location of bits that are allocated to files.

#### Data Structure of the FAT file system

##### Clusters
Each file stored on a storage device can be considered a group of clusters containing bits of information.
##### Directory
Contains information about file identificaton such as file name, starting cluster, filename length.
##### File Allocation Table
Linked list of all the clusters containing the clusters status and a pointer to the next cluster.

There are a number of different iterations of the FAT file system. FAT12, FAT16, and FAT32. 

#### exFAT

Due to the limitations of FAT32, exFAT was created and is the default file system for most media devices. It supports 4KB to 32MB cluster sizes, and a maximum volume of 128PB. It is also a mucher lighter more efficient option due to the reduction of a lot of the overheads from the FAT file system.

### NTFS File System

NTFS or **New Technology File System** replaced FAT as the default file system in Windows.

##### Journaling
Changes to metadata in the NTFS volume are stored in a file named ```$LOGFILE``` which is located in the root directory.
##### Access Controls
NTFS has access controls that control the use of files and directories based on the user.
##### Volume Shadow Copy
The Volume Shadow Copy keeps track of changes to the NTFS file system. Users can use this to restore files to previous versions.
##### Alternate Data Streams
Allows files to have multiple streams of data stored in a single file. Malware has been observed to hide its code in ADS.
##### Master File Table
The MFT is a structured database that tracks objects stored in a volume. NTFS data is organised in the Master File Table. Some important places to look at in the MFT are:
* $MFT - First record in the volume. Contains a directory about all the files present on the volume.
* $LOGFILE - Stores the transactional logging of the file system.
* $UsnJrnl - Update Sequence Number Journal. Found in the $External record, contains information about all the files that were changed as well as the reason for the change.
* MFT Explorer - Eriz Zimmerman tool used to explore MFT files. 

### MFTExplorer Usage

For the command line version of this tool we can use the following command to create a csv output file of a MFT file:
* ```MFTECmd.exe -f <path-to-mft-file> --csv <path-to-save-results-in>```
For the practical lab I ran the following command to make a csv output file of the $MFT file which I can view in EZViewer:
* ```MFTECmd.exe -f C:\Users\THM-4n6\Desktop\Triage\C\$MFT --csv C:\Users\THM-4n6\Desktop\mftoutput.csv```

### Recovering Deleted Files

We can use a tool named **Autopsy** to recover deleted files from a volume. This is a GUI application that allows us to import a copied drive and analyse the file system on it, including seeing deleted files which we can then attempt to recover.

### Evidence of Execution

The following are artifacts that provide evidence of execution.

##### Windows Prefetch Files
Windows stores information about files so that it can speed up the execution of the file for future use. These are called prefetch files and are located in the prefetch directory at ```C:\Windows\Prefetch```. They have a file extension of ```.pf```. For the parsing of prefetch files, we can use **Prefetch Parser (PECmd.com** from Eric Zimmermans tools. This will allow us to extract and parse prefetch files. Usage for this tool is as follows:
* For parsing a single file - ```PECmd.exe -f <path-to-Prefetch-files> --csv <path-to-save-csv>```
* For parsing a directory - ```PECmd.exe -d <path-to-Prefetch-directory> --csv <path-to-save-csv>```

##### Windows 10 Timeline
Recently used applications and files are stored in an SQLite database known as the Windows 10 Timeline. We can find this information at ```C:\Users\<username>\AppData\Local\ConnectedDevicesPlatform\{randomfolder}\ActivitiesCache.db```.
The Windows 10 timeline can be parsed with a tool by Eric Zimmerman named **WxTCmd.exe**. Our usage of this tool would look like:
* ```WxTCmd.exe -f <path-to-timeline-file> --csv <path-to-save-csv>```

##### Windows Jump Lists
Jump lists are the lists of recently accessed files we see when right-clicking a application on the taskbar. Jump lists can be found at ```C:\Users\<username>\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations```.
Again, Zimmerman has a tool for parsing Jump Lists called **JLECmd.exe**. Usage would look something like:
* ```JLECmd.exe -f <path-to-Jumplist-file> --csv <path-to-save-csv>```

### File/Folder Knowledge

##### Shortcut Files
For each file that is opened either locally or remotely, Windows creates a shortcut for that file. These shortcuts are stored at:
* ```C:\Users\<username>\AppData\Roaming\Microsoft\Windows\Recent\```
* ```C:\Users\<username>\AppData\Roaming\Microsoft\Office\Recent\```
Zimmermans **LECmd.exe (LnkExplorer)** to parse Shortcut files. Our usage would something like:
* ```LECmd.exe -f <path-to-shortcut-files> --csv <path-to-save-csv>```

###### IE/Edge History
IE/Edge history also includes files opened in the system, whether those files were opened using the browser or not. This history can be found at:
* ```C:\Users\<username>\AppData\Local\Microsoft\Windows\WebCache\WebCacheV*.dat```
We can use **Autopsy** to view Web cache data. Usage for Autopsy to view IE/Edge history is as follows:
* Logical files as data source
* Provide the path to the data
* Check Recent Activity and uncheck everything else
* View logical files in the Web History option in the left panel

##### Jump Lists
Jumplists can be used to identify the last opened files or last executed programs in a system.

### External Devices/USB Device Forensics

##### Setupapi Dev Logs For USB Devices
All information related to new devices that are attached to the system gets stored in ```setupapi.dev.log```. We can find this log file at:
* ```C:\Windows\inf\setupapi.dev.log```

##### Shortcut Files
Shortcuts can sometimes give us an insight into connected USB devices. It can provide us with information about the volume name, type, and serial number. Again, we can find shortcut information at:
* ```C:\Users\<username>\AppData\Roaming\Microsoft\Windows\Recent\```
* ```C:\Users\<username>\AppData\Roaming\Microsoft\Office\Recent\```




