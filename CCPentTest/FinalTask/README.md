# CCPenTest Final Room - THM
### donkeyk0ng787/gnome787

#### TARGET IP - 10.10.64.41

### Enum

	'Found hidden directory /secret'
	'Found secret.txt inside the /secret directory'
	'File contains the following
	nyan:046385855FC9580393853D8E81F240B66FE9A7B8'
	'The hash is a sha1 which we can crack with hashcat'
	'Hashcat result - 046385855fc9580393853d8e81f240b66fe9a7b8:nyan'

### Creds

	ssh = nyan:nyan

### Priv Esc

	'Cat display the shadow file'
	'Doing sudo -l shows us that nyan can run /bin/su as sudo
	so we can use that to login to the root acount by doing the following
	sudo /bin/su'
	'We now have root

### Flags

	'User'

	'supernootnoot

	'Root'

	'congratulations!!!!'
