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

For hashed versions of users passwords ```/etc/shadow```

### Privileges

The file privileges will look something like the following ```-rwxrw-r-x```. In this example the item is a file as is denoted by the ```-```, the file owner has read, write and execute permissions as is denoted by the ```rwx``` part, the group owner of the file has read and write permissions as is denoted by the ```rw-``` and all other users have read and execute permissions as is denoted by the ```r-x```.

### Configuration Files

Gather DNS information ```/etc/hosts```

Gather DNS server information ```/etc/resolv.conf```

The **inetd** daemon's config file can be found at ```/etc/inetd.conf  ```. This file contains a list of network services that **inetd** should listen for and launch upon request from clients.

### Network Information

Gather open ports and services ```netstat -natp```

### Logs

Syslog contains messages about  system activity. The detail of the recorded logs can be configured through the logging level. It is found at ```/var/log/syslog```.

Syslog customisation can be done in the ```syslog.conf``` file found in the ```/etc``` directory.

Tracking of user logins is stored in ```/var/log/lastlog```. This can be accessed on a live system via the command ```lastlog```.

Login information can be found at ```/var/log/auth.log```. A useful command to filter out useful information is ```grep -v cron /var/log/auth.log*|grep -v sudo|grep -i user```.

To open the following 2 files, you need to use the last command as they are binary files not text files. For example ```last -f /var/log/wtmp```.

Retrieve system status, reboot time, and user logins information. ```/var/log/wtmp```.  

Failed login attempts ```/var/logbtmp```.

Often linux systems are used as webservers. The logs location for a linux based webserver is generally ```/var/log/apache2/accesslog```. In the case that they are not there, simply cd into ```/var/log``` and run the following command ```find . -type f -name "*access.log*"```. If there are any access logs, that find command should locate them.

### Process Information - Live system

List all process information ```ps aux```

Print a processs tree ```ps axjf```

Show all running processes ```ps -e```

Show all processes spawned from the current user ```ps -u```

View systemd log information with ```jounralctl```

### Persistence Mechanisms

To view the scheduled tasks on a linux system, aka **Cron Jobs**, ```cat /etc/crontab```

Linux (like Windows) has the ability got services to start and run in the background after every system boot. These can be found at ```/etc/init.d```.

The ```~/.bashrc``` file contains terminal session configurations that are executed when a user logs in. 

### Evidence of Execution and Application Usage

A log of all the commands that are run on a system as **sudo** can be found at ```/var/log/auth.log*```. A useful one-liner to retrieve these commands is ```cat /var/log/auth.log* | grep -i COMMAND | tail```

All other commands that are run without using sudo are stored in the ```~/.bash_history``` file.

Vim stores logs for opened files in Vim in the ```~/.viminfo``` file.

Recently opened docs can be found at ```~/.recently-used``` or ```~/local/share/recently-used.xbel```

Note: Different shell types will have their own history file. These are generally still found in the home directory of the user and will typically follow the format ```~/.*_history```

Historical use of the command ```less``` may exist in user home directories at ```~/.lesshst```

Information about files that were recently accessed using applications running in the Gnome desktop can be found at ```~/.recently-used.xbel```.

##### SSH

SSH connection evidence may be found in the following file ```~/.ssh/authorised_keys```

Another SSH path that may contain information about known hosts is ```~/.ssh/known_keys```

### Filesystem Information

View file system information at ```/etc/fstab```

Display file or filesystem status with ```stat```. Example ```stat /etc/resolv.conf```.

Find more detailed information about a file with the following ```sudo debugfs -R 'stat /etc/resolv.conf' /dev/sda1```

Find the root filesystem - ```df -h```.

On debian systems, you can view details about installed packages at ```/var/lib/dpkg/status```. Information about when packages were installed can be found at ```/var/log/dpkg.log```. 

### Browser Artifacts

Note - the path for the various Firefox artefacts may vary depending on the version of Firefox that is installed. For example, Kali uses **Firefox ESR** which has a path that looks like ```~/.mozzila/firefox/<random-string>.default-esr/```. Therefore, it is important to look around the ```~/.mozzila/fiefox/``` directory as the artefacts will be there, they just might be in a slightly different directory to the ones expressed below.

```~/.mozilla/firefox/<random_string>.default/places.sqlite``` Firefox bookmark database file and history
```~/.mozilla/firefox/<random_string>.default/cookies.sqlite``` SQLite database containing information about website cookies
```~/.mozilla/firefox/<random_string>.default/formhistory.sqlite``` SQLite database containing information about form entries
```~/.mozilla/firefox/<random_string>.default/sessionstore.jsonlz4``` JSON file containing information about open tabs and windows
```~/.mozilla/firefox/<random_string>.default/key4.db``` SQLite database containing encrypted form data, passwords, and certificates
```~/.mozilla/firefox/<random_string>.default/cert9.db``` SQLite database containing SSL certificates
```~/.mozilla/firefox/<random_string>.default/signons.sqlite``` SQLite database containing usernames and passwords
```~/.mozilla/firefox/<random_string>.default/extensions/``` directory containing installed Firefox extensions
```~/.mozilla/firefox/<random_string>.default/bookmarkbackups/``` directory containing backups of Firefox bookmarks
```~/.mozilla/firefox/<random_string>.default/permissions.sqlite``` SQLite database containing permissions granted to websites
```~/.mozilla/firefox/<random_string>.default/addons.json``` JSON file containing information about installed add-ons
```~/.mozilla/firefox/<random_string>.default/search.json.mozlz4``` compressed JSON file containing information about search engines
```~/.mozilla/firefox/<random_string>.default/formfill.sqlite``` SQLite database containing information about form entries

``` ~/.config/google-chrome/Default/History``` Google Chrome browser history. ```~/.config/google-chrome/Default/Bookmarks``` Bookmarks saved in Google Chrome.
```~/.config/google-chrome/Default/Cookies``` Cookies saved by Google Chrome.
```~/.config/google-chrome/Default/Login Data``` Encrypted login credentials saved by Google Chrome.
```~/.config/google-chrome/Default/Preferences``` User preferences for Google Chrome.
```~/.config/google-chrome/Default/Top Sites``` List of top sites visited in Google Chrome.
```~/.config/google-chrome/Default/Web Data``` SQLite database containing autocomplete form data, keywords and search terms.







