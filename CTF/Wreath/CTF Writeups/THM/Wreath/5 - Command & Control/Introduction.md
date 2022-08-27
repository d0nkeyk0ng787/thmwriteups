# Introduction

Now that we have a stable connection into the **10.200.196.150** system, we can start to bring a C2 framework into play. Making use of a C2 framework is extremely useful, it will allow us to increase the efficiency of, and simplify, certain aspects of our attack (pivoting, AV evasion, privesc, looting etc )

For our target, we are going to use **Powershell Empire** which is a framework that is designed primarily for attacking Windows machines. Powershell Empire provides us as the attacker the ability to take initial access to a network of devices and turn it into something much bigger. Using Powershell Empires GUI interface (Starkiller) we can improve our shell and perform post-exploitation on our compromised Windows server

