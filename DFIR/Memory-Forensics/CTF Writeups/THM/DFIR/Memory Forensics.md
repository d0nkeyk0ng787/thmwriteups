# Memory Artefacts

### MemProcFS

MemProcFS can be used to mount memory images along with that images corresponding pagefile and swapfile. It also has a number of other functionality that can aid in a forensic examination of a memory image.
##### Windows
The following commands would be performed in a Windows environment:
```posh
# Note: All these commands have "-v" at the end for a more verbose terminal output. This is optional but recommended
# Mount a memory image
.\MemProcFS.exe -device <path-to-memory-image> -v
# Mount a memory image and a pagefile
.\MemProcFS.exe -device <path-to-memory-image> -pagefile0 <path-to-pagefile> -v
# Mount a memory image, pagefile, and a swapfile
.\MemProcFS.exe -device <path-to-memory-image> -pagefile0 <path-to-pagefile> -pagefile1 <path-to-swapfile> -v
# Example 
.\MemProcFS.exe -device C:\Users\Administrator\Documents\memdump.raw -pagefile0 C:\Users\Administrator\Documents\pagefile.sys -pagefile1 C:\Users\Administrator\Documents\swapfile.sys -v
```

More information about MemProcFS can be found on the wiki. [MemProcFS Wiki](https://github.com/ufrisk/MemProcFS/wiki/_Linux)

##### Linux
The following command would be performed in a Linux environment:
```bash
# To Be Added
```

### MemProcFS Analyser

MemProcFS analyser is a powershell script that has the capability to mount and image file along with its corresponding pagefile and then execute a number of forensic tools on that image to produce output that would be useful in a forensic examination of memory. To run MemProcFS analyser simply do the following:
```posh
# From Windows terminal or powershell, cd into the install dir for MemProcFS-Analyser and run it like so
.\MemProcFS-Analyser.ps1
# From here you will be presented with a GUI where you will input the path to your memory dump and optionally enter the path to its corresponding pagefile.
# Following this MemProcFS-Analyser will create an output directory where it will dump a number of useful artefacts to aid in your forensic examination of that memory image.
```

### Volatility

The following are a number of useful Volatility commands for analysing a memory image. Where possible I have included the commands for both Volatility2 and Volatility3.

Note - This writeup is focused on Windows memory images, however, some of these commands might produce output for linux memory images. Further, all usage of volatility3 requires python3 and all usage of volatility2 requires python2.

##### Gather Basic Information
Produce information about the image being analysed
```posh
# Volatility3
python .\vol.py -f <path-to-memory-image> windows.info
```
```bash
# Volatility2
python2 ./vol.py -f <path-to-memory-image> imageinfo
```

Gather process information
```posh
# Volatility3
python .\vol.py -f <path-to-memory-image> windows.pslist
python .\vol.py -f <path-to-memory-image> windows.psscan
# pstree obviously outputs the process in a tree format showing parent-child process relationships
python .\vol.py -f <path-to-memory-image> windows.pstree
```
```bash
# Volatility2
python2 ./vol.py -f <path-to-memory-image> pslist
python2 ./vol.py -f <path-to-memory-image> pscan
# pstree obviously outputs the process in a tree format showing parent-child process relationships
python2 ./vol.py -f <path-to-memory-image> pstree
# psxview attempts to display hidden processes
python2 ./vol.py -f <path-to-memory-image> psxview
```

Gather network information
```posh
# Volatility3
python .\vol.py -f <path-to-memory-image> windows.netstat
python .\vol.py -f <path-to-memory-image> windows.netscan
```
```bash
# Volatility2
python2 ./vol.py -f <path-to-memory-image> netscan
# For Windows XP systems use
python2 ./vol.py -f <path-to-memory-image> connscan
python2 ./vol.py -f <path-to-memory-image> sockscan
```

##### Identify Malware In Processes
Search the memory image for potentially malicious code
```posh
# Volatility3
python .\vol.py -f <path-to-memory-image> windows.malfind
```
```bash
# Volatility2
python2 ./vol.py -f <path-to-memory-image> malfind 
```

##### Analyse Specific "Suspicious" Processes
List DLL's that are loaded into specific processes
```posh
# Volatility3
python .\vol.py -f <path-to-memory-image> windows.dlllist --pid <pid>
```
```bash
# Volatility2
python2 ./vol.py -f <path-to-memory-image> dlllist -p <pid>
```

Locate suspicious DLLs in a process. A good indicator of a suspicious DLL is if **InLoad**, **InInit**, and **InMem** are all **FALSE**.
```posh
# Volatility3
python .\vol.py -f <path-to-memory-image> windows.ldrmodules --pid <pid>
```
```bash
# Volatility2
python2 ./vol.py -f <path-to-memory-image> ldrmodules -p <pid>
```

Locate handles related to a specific process
```posh
# Volatility3
python .\vol.py -f <path-to-memory-image> windows.handles --pid <pid>
```
```bash
# Volatility2
python2 ./vol.py -f <path-to-memory-image> handles -p <pid>
# The benefit of Volatility2 here is that you can specify it to only print specific handle types like so:
python2 ./vol.py -f <path-to-memory-image> handles -p <pid> -t <type>
# Types includes File, Event, Key, Port
```

##### General Commands

View recent commands
```posh
# Volatility3
python .\vol.py -f <path-to-memory-image> windows.cmdline
```
```bash
# Volatility2
python2 ./vol.py -f <path-to-memory-image> cmdline
```

Interact with the registry
```posh
# Volatility3
# List registry hives in the memory image
python .\vol.py -f <path-to-memory-image> windows.registry.hivelist
# Scan for active registry hives and extract registry keys values
python .\vol.py -f <path-to-memory-image> windows.registry.hivescan
# Print a list of registry keys
python .\vol.py -f <path-to-memory-image> windows.registry.printkey
# Print a specific registry key
python .\vol.py -f <path-to-memory-image> windows.registry.printkey --key <path-to-key>
```
```bash
# Volatility2
python2 ./vol.py -f <path-to-memory-image> hivelist
python2 ./vol.py -f <path-to-memory-image> hivescan
# Dump registry files out to disk
python2 ./vol.py -f <path-to-memory-image> dumpregistry --dump-dir <path-to-dump-dir>
python2 ./vol.py -f <path-to-memory-image> printkey --key <path-to-key>	
```

Additional commands and usage information can be found at the following links. 
* [Volatility2 Wiki](https://github.com/volatilityfoundation/volatility/wiki) 
* [Volatility3 Documentation](https://volatility3.readthedocs.io/en/latest/)