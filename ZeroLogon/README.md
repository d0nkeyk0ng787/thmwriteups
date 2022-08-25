# Zero Logon - THM Room

### Gnome787 | 25 AUG 22

### The Zero Day Angle

Zero Logon is a vulnerability that allows an attacker to go from Zero to Domain Admin in roughly 1 minute. It abuses a feature within Microsoft NetLogon Remote Protocol (MS-NRPC) which is a critical authentication component of Active Directory. MS-NRPC is responsible for handling the authentication of User and Machine accounts within Active Directory. 

Zero Login is able to abuse this feature mainly due to poor implemention of cryptography on the part of Microsoft. Essentially, Microsoft chose to use AES-CFB8 for a function called ComputeNetLogonCredential, and instead of using a random string for the initialisation vector, they hard coded it in as all zeroes. This meant that when an attacker sent a message with an initialisation vector containing all zeroes, there is a 1 in 256 chance that the Ciphertext will be a zero. 

This vulnerability can be used to bypass authentication on the Domain Controller machine account, from there we could use a tool like 'SecretsDump.py' to dump all the passwords within the domain. From here you coluld attempt to crack the password hashes and continue owning the system.

The following is a breakdown of where the vulnerability occurs in the MS-NPRC logon process:
1. The client creates and sends a 'NetServerReqChallenge' which contains the DC, the target device, a Nonce (This would be your 16 bytes of zeroes)
2. After receiving the NetServerReqChallenge, the server will generate and send it's own Nonce (this is called the Server Challenge) back to the client
3. From here the client computes it's NetLogon Credentials with the Server Challenge provided. The client will use the 'NetServerAuthenticate3' method which requires the following parameters:
	1. A Custom Binding Handle (Impacket handles this for us, it's negotiated prior)
	2. An Account Name (The Domain Controller's machine account name. ex: DC01$)
	3. A Secure Channel Type (Impacket sort of handles this for us, but we still need to specify it: [nrpc.NETLOGON_SECURE_CHANNEL_TYPE.ServerSecureChannel])
	4. The Computer Name (The Domain Controller ex: DC01)
	5. The Client Credential String (this will be 8 hextets of \x00 [16 Bytes of Zero])  
	6. Negotiation Flags (The following value observed from a Win10 client with Sign/Seal flags disabled: 0x212fffff Provided by Secura)
4. After the client sends the 'NetServerAuthenticate' request, the server will compute that same request itself using it's known, good values. If it determines that the rquest is legitimate, the server will send the client the required information.
NOTE: From here, steps 3 and 4 will be looped through a number of times until the server has the same computations as the clients which is a 1-in-256 chance.
5. If the server calculates the same value as the client, the client will re-verify, after mutual agreement between the server and client, they will agree on a session key which will be used to encrypt communications between the client and server.

### Proof of Concept

For this, we will be using a PoC created by someone named Secura which can be found here 'https://raw.githubusercontent.com/SecuraBV/CVE-2020-1472/master/zerologon_tester.py'.

Questions
1. NetrServerPasswordSet
2. PrimaryName,AccountName,SecureChannelType,ComputerName,Authenticator,ReturnAuthenticator,ClearNewPassword
3. 30

### Lab it Up!

To find the answer to the first 3 questions I simply ran a nmap scan like so 'nmap -sC -sV 10.10.182.111'

From there I used the name 'DC01' and IP '10.10.182.111' to run the Zero Logon exploit which changed the machine accounts password. Then I used 'secretsdum.py' to retreive said password. I did have to do this in the impacket virtualenv we created earlier. I have a guide for that in my guides folder.

From there we got the Administrator accounts NTLM hash which we can use with Evil-Winrm to login and get further exploit our target. 

Connecting to the system with evil-winrm is done like so 'evil-winrm -u Administrator -H 3f3ef89114fb063e3d7fc23c20f65568 -i 10.10.182.111 '

Using the powershell command 'Get-AdGroupMember 'domain admins' we can return all users who are members of the AD group domain admins 

From here we find the root flag on the Admins desktop and we are done

Questions
1. DC01
2. HOLOLIVE
3. hololive.local
4. 3f3ef89114fb063e3d7fc23c20f65568
5. 2
6. THM{Zer0Log0nD4rkTh1rty}


### Summary

In summary, Zero Logon is a powerful exploi that abuses poor cryptography implementation in Microsofts NetLogon Remote Protocol (MS-NPRC) which handles authentication between machine accounts and users within active directory.
