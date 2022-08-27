# Empire: Stagers

Stagers are Empire's payloads that create a connection back to the listener which creates an agent upon execution

Stagers will, most of the time, be given as script files which get uploaded to the target system and get executed. Empire is really good as it gives us a wide range of options for creating and obsfuscating our payload to avoid AV detection

### Creating a stager

Heading into the **Stagers** menu, we can click the orange create button and a similar form pops up that we fill out in the same way we did for the **Listeners** form. For the type we want **multi/bash**, give it a name and have it set to the listener we created. From here we can copy the payload to our clipboard by clicking the three dots under the **Actions** section

