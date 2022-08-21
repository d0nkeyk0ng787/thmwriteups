# Hacking With Powershell - THM Room

### Enumerating Windows With Powershell

Enumerating Users - Get-LocalUser
Find a users SID - Get-LocalUser | Select name,sid
Find users that have their required password value set to false - Get-LocalUser | Where-Object -Property RequirePassword -Match False
Find groups - Get-LocalGroup
Get IP address info - Get-NetIPAddress
Find listening ports (and return the count) - Get-NetTCPConnection | Where-Object -Property State -Match Listen | measure
Find how many patches have been applied - get-HotFix | measure
Find contents of a backup file - Get-ChildItem -Pat C:\ -Include '*.bak*' -File -Recurse
Get-Content 'C:\FILEPPATH'
List all processes - Get-Process
Find a scheduled task by name - Get-ScheduledTask -TaskName NAME
Find the owner of a file - Get-Acl PATH

Script to find a password

$path = Get-Content -Path C:\Users\Administrator\Desktop\emails
$pattern = 'password'
$command = Get-ChildItem -Path $path -Recurse | Select-String $pattern
echo $command

Find a https link

Note - Simply replace the $pattern variable of 'password' to 'https'

$path = Get-Content -Path C:\Users\Administrator\Desktop\emails
$pattern = 'https'
$command = Get-ChildItem -Path $path -Recurse | Select-String $pattern
echo $command
