# Introduction

We are going to cover some fundamentals when it comes to AV evasion. This is primarily because the field is so dynamic and is constantly being updated, it's impossible to cover the latest and greatest methods and expect them to stay the latest and greatest beyond a fairly short period of time

There are two primary types of AV evasion:
 - On-disk evasion: Involves getting a file onto the target and then executing it
 - In-memory evasion: Involves importing a script directly into the memory and then executing it there. This may be a powershell module we get from the web or our own device

In-memory has very much been the pick of the two as AV software has been unable to scan scripts as they enter the memory. This has changed with the development of the Anti-Malware Scan Interface (AMSI) developed by Microsoft. This tool is able to scan the script as it enters the memory and create hooks that existing AV software can use to scan the script and see if it is malicious

We would typically start off by trying to determine what AV is on the targets system. This would most likely be done through the use of social engineering and therefore won't be part of this attack so for now we will assume the system is running the standard Windows defender. If we already have a shell, we can skip having to do any SE and possibly use tools such as **SharpEDRChecker** and **Seatbelt** to identify the AV solution installed. From there, we would ideally have the OS and AV versions and would create a replica of our target in a virtual environment to begin trying to evade the AV and gain access. We want to make sure we disconnect the AV softwares ability to report our attacks back to their servers for analysis so we need to disable the AV softwares cloud-based protection or we could simply disconnect the system from the internet

AV Evasion usually involves some form of obfuscation when it comes to payloads. This could mean anything from moving things around in the exploit and changing variable names, to encoding aspects of the script, to outright encrypting the payload and writing a wrapper to decrypt and execute the code section-by-section. The aim is to switch things enough that the AV software is unable to detect anything bad.