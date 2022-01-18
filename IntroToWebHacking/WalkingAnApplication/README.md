# Walking An Application - THM Room
### d0nkeyk0ng787/gnome787

### Task 2 - Exploring The Website

To begin with, you more or less just want to have a look around the site and get an overview of what it is doing and attempt to discover any potential exploitation parts. This may include website forms you may be able to exploit or perhaps it is running a plugin or app that has a known bug or perhaps one can be found.

### Task 3 - Viewing The Page Source

Viewing the page source is a very useful method of recon when it comes to web app exploitation. You can see how the site functions, perhaps find hidden pages and read comments that a dev may have forgotten to remove which may contain useful info such as credentials

For this task there are 4 flags we can get

The first one can be gotten simply by opening the page source and looking at the first comment which has the page name of the future index page

Flag 1: THM{HTML_COMMENTS_ARE_DANGEROUS}

The second flag is found further down in the code, we see a page named /secret which we can open and there is the second flag

Flag 2: THM{NOT_A_SECRET_ANYMORE}

The third flag can be found by enumerating the site a little. We see in the code that the site refers to a '/assets/' folder a few times to add some CSS to the site. If we head to this location we can get the third flag

Flag 3: THM{INVALID_DIRECTORY_PERMISSIONS}

This last one can be found by enumerating the framework that the site is built on. By heading to the framework link that is commented in the source code of the index page we can look around and in the changelog section it makes reference to a /tmp.zip file. If we head to this location on our site it attempts to download a zip file which contains the flag inside

Flag 4: THM{KEEP_YOUR_SOFTWARE_UPDATED}

### Task 4 - Developer Tools: Inspector

Modern browsers include developer tools which can be used when pentesting a web app to see what is happening and how

The inspector is a useful tool for pentesters as it allows for the modification of a site (client-side only) to tinker around and see if you can get it to do something it shouldn't or expose something it shouldn't

In the case of our site we have a news page containing 3 articles. The third article is blocked by a paywall. Taking a look at that paywall in the inspector, we can see in the elements tab that there are a number of styles. Of interest to us is the style named 'display' with a value of block. From here we can modify this variable to 'none' which removes that paywall and exposes the article as well as the flag

Flag: THM{NOT_SO_HIDDEN}

### Task 5 - Developer Tools: Debugger

The debugger is another useful tool a pentester can use as it allows us to delve into the javascript code on the site and get an idea of what is occuring

Often web devs will minimise a js file meaning everything is then on one line. This can be fixed by clicking the two curly braces at the bottom known as 'pretty print'. This will reformat the file correctly.

Often web devs will also obsfuscate a js file making it harder to ascertain what is going on. Regardless when taking a look at our contact page we can still get a vague idea.

In the flash.min.js file we see that there is a line towards the bottom that may be causing our red flash 'flash['remove'](); '

By clicking the line number we can create a break in the code that will tell the compiler to stop processing the js and pause the execution

We create our breakpoint and refresh the page and get our flag

Flag: THM{CATCH_ME_IF_YOU_CAN}

### Task 6 - Developer Tools: Network

The last of the dev tools that is of interest to a pentester is the Network tool. This tool can be used to see external web requests by the site. By opening the network tab and refreshing the page you can see all the sites your web page will make a request to

Given that our contact page has form we can tinker with, if we apply some input to it, we see that it makes a POST request to a '/contact-msg' page

When we visit this page we can get the flag

Flag: THM{GOT_AJAX_FLAG}
