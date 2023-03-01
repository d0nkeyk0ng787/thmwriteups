# Additional Windows Artefacts

### Registry

##### SAM



##### SOFTWARE

Information regarding registered owner, registered organisation, product name, install date. Install date is in hex/decimal and needs to be decoded with a tool like **dcode.exe**. 
```SOFTWARE\Microsoft\Windows NT\CurrentVersion```

Windows Vista. If a USB device has been enabled as a ReadyBoost device, it will have the **Drive Status** value in the following registry key set to **2**.
```SOFTWARE\Microsoft\Windows NT\CurrentVersion\EMDMgmt```

##### SYSTEM

See if NtfsDisablelastAccessUpdate is enabled or disabled.
```SYSTEM\CurrentControlSet\Control\FileSystem```

Using usbdeview to view usb devices in a SYSTEM file. Run the following command on the SYSTEM file.
```.\USBDeview.exe /regfile .\SYSTEM```

##### NTUSER.dat

Most recent Win + R commands. Entries are assigned letters of the alphabet in descending order with the most recent command being the closest letter to z.
```SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\RunMRU```

