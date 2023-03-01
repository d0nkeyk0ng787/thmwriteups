# Linux Artefacts

### System Info

System information ```/etc/os-release```

Kernel and build information  can be found at ```/proc/version``` or by running the command ```uname``` with various switchers such as ```-r``` or ```-a```.

Timezone information can be found at ```/etc/timezone```

The hostname can be found at ```/etc/hostname```

Interfaces information can be found at ```/etc/network/interfaces```

Use the which command on a service to find its full path. Example ```which vim```.

### User Info

User information can be found at ```/etc/passwd```

Group information can be found at ```/etc/groups```

Sudoers information can be found at ```/etc/sudoers```

### Privileges

The file privileges will look something like the following ```-rwxrw-r-x```. In this example the item is a file as is denoted by the ```-```, the file owner has read, write and execute permissions as is denoted by the ```rwx``` part, the group owner of the file has read and write permissions as is denoted by the ```rw-``` and all other users have read and execute permissions as is denoted by the ```r-x```.

### Configuration Files

Gather DNS information ```/etc/hosts```

Gather DNS server information ```/etc/resolv.conf```

### Network Information

Gather open ports and services ```netstat -natp```

### Logs

Syslog contains messages about  system activity. The detail of the recorded logs can be configured through the logging level. It is found at ```/var/log/syslog```.

Login information can be found at ```/var/log/auth.log```. A useful command to filter out useful information is ```grep -v cron /var/log/auth.log*|grep -v sudo|grep -i user```.

To open the following 2 files, you need to use the last command as they are binary files not text files. For example ```last -f /var/log/wtmp```.

Retrieve system status, reboot time, and user logins information. ```/var/log/wtmp```.  

Failed login attempts ```/var/logbtmp```.

### Process Information

List all process information ```ps aux```

Print a processs tree ```ps axjf```

Show all running processes ```ps -e```

Show all processes spawned from the current user ```ps -u```

View systemd log information with ```jounralctl```

### Persistence Mechanisms

To view the scheduled tasks on a linux system, aka **Cron Jobs**, ```cat /etc/crontab```

Linux (like Windows) has the ability got services to start and run in the background after every system boot. These can be found at ```/etc/init.d```.

The ```~/.bashrc``` file contains terminal session configurations that are executed when a user logs in. 

### Evidence of Execution

A log of all the commands that are run on a system as **sudo** can be found at ```/var/log/auth.log*```. A useful one-liner to retrieve these commands is ```cat /var/log/auth.log* | grep -i COMMAND | tail```

All other commands that are run without using sudo are stored in the ```~/.bash_history``` file.

Vim stores logs for opened files in Vim in the ```~/.viminfo``` file.

### Filesystem Information

View file system information at ```/etc/fstab```

Display file or filesystem status with ```stat```. Example ```stat /etc/resolv.conf```.

Find more detailed information about a file with the following ```sudo debugfs -R 'stat /etc/resolv.conf' /dev/sda1```

Find the root filesystem - ```df -h```.





