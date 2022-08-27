# AV Detection Methods

AV has different detection methods so that it is able to pick up on different kinds of malware that use different techniques to exploit the target and evade detection

Detection methods can generally be classified as either:
- Static detection
- Dynamic / Heuristic / Behavioural Detection

Typically, modern AV software will rely on a combination of these

### Static Detection Methods

Typically, static detection methods involve signature detection. A common but fairly easy to bypass use of static detection is comparing the hashsum of a file and comparing it to a database of known malware hashsums. This is very easy to bypass as a simply change to the malware code will change this hashsum

Byte matching is another form of static detection that is commonly used in AV software and is significantly more effective then the previous method. Byte matching involves searching through the code and taking a byte sequence and then comparing that byte sequence against a known database of bad bytes. Whilst effective, this method is quite slow, therefore it's common for AV software to only hash small sections of the file to check which obviously reduces the effectiveness of this method

### Dynamic Detection

Dynamic detection methods involve looking at how the executable behaves. That is to say, what is the file doing on the system? Is it trying to connect to a suspicious website or server? Is it adjusting things in the registry that it shouldn't be. Essentially it is looking to see how the file intends to act and make a decision as to whether it should quarantine it 

One other dynamic method that is used by AV software to detect malware is by running it in a sanboxed environment under close supervision by the AV which will then either determine if it should quarantine and flag it as malware or allow it to run

Evading a sandbox environment may involve trying to detect if it is running in a virtualised environment by checking to see if the system has a fan, GUI and is running a classic VM service and if so exiting

Making the malware function in a way that is unexpected may also evade the AV detection. This may be something as simple as adding in redundant code to throw the AV off. Alternatively, doing something as simple as password protecting the file may beat the AV detection

Dynamic detection solutions tend to take significantly more time to complete and therefore are often not used in the most effective manner. Regardless, using dynamic and static detection methods in conjunction with one another, as well as new techniques that are being developed and released all the time, makes AV software very effective at detecting malware

