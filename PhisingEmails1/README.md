# Phishing Emails 1 Room THM
### Completed 22:12 31 DEC 21
### d0nkeyk0ng787/gnome787

#### BOX IP - 10.10.151.119

### Task 1 - Introduction

	-
	
### Task 2 - The Email Address

	The concept of emails dates back to the 1970s and is attributable to a man named Ray Tomlinson who designed it for ARPANET.
	
	An email address is made of of:
		- User Mailbox (or Username)
		- @
		- Domain
		
	To break down an email to each of its components, we will take bill@johndoe.com
		- The User Mailbox or username is 'billy'
		- The @ is fairly straight forward
		- The domain is of course 'johndoe.com'
		
	Question Answers
		'1970s'
		
### Task 3 - Email Delivery

	There are 3 protocols at work that facilitate the outgoing and incoming email messages. These are:
		- SMTP (Simple Messaging Transfer Protocol): Handles the sending of the email
		- POP3 (Post Office Protocol): Transfers an email between client and mail server
		- IMAP (Internet Message Access Protocol): Transfer an email between client and mail server
		
		Note: Whilst POP3 & IMAP have the same definition, there are differences between the two
		
		POP3
		    Emails are downloaded and stored on a single device.
		    Sent messages are stored on the single device from which the email was sent.
		    Emails can only be accessed from the single device the emails were downloaded to.
		    If you want to keep messages on the server, make sure the setting "Keep email on server" is enabled, or all messages are deleted from the server once downloaded to the single device's app or software.

		IMAP
		    Emails are stored on the server and can be downloaded to multiple devices.
		    Sent messages are stored on the server.
		    Messages can be synced and accessed across multiple devices.
		    
	The process of sending and recieving an email is as follows:
			1. An email is composed and the send button is hit
			2. The SMTP protocol needs to know where to send the email so it queries the DNS server to find out where the emails domain is located
			3. This information is obtained by the DNS server and forwarded to the SMTP client
			4. The SMTP client sends the composed email accross the internet to the specified address
			5. The email passes through various SMTP servers and is finally relayed to the destination SMTP server
			6. The email finally reaches the destination SMTP server
			7. The email is forwarded to the local POP3/IMAP server waiting for the recipient
			8. The recipient logs into his email client, which queries the local POP3/IMAP server for new emails
			9. The email is then copied (IMAP) or downloaded (POP3) to the recipients email client
			
	Question Answers
		'465'
		'993'
		'995'
		
### Task 4 - Email Headers

	It's important to understand the components that make up an email as it aids in analysing potentially malicious emails
	
	There are two main parts that make up an email. They are:
		- the email header (information about the email, such as the email servers that relayed the email)
		- the email body (text and/or HTMl formatted text)
		
	Quick note, the syntax for email messages is known as Internet Message Format (IMF). It is outlined in RFC (Request For Comments) 5322 which originally derived from RFC 2822, which itself came from RFC 822.
	
	When analysing an email header, there are some key things you look for. They are:
		- From: The sender's email address
		- Subject: The email's subject line
		- Date: The date the email was sent
		- To: The recipient's email address
		
	You can get all this information from just looking at the email in your inbox or you can view the email in 'raw mode' and get a lot more information in a less user friendly format
	The following link is a useful writeup on viewing the raw/full email headers in various email clients:
	https://mediatemple.net/community/products/grid/204644060/how-do-i-view-email-headers-for-a-message
	
	Another useful link from mediatemple is a breakdown on how to analyse email headers:
	https://mediatemple.net/community/products/all/204643950/understanding-an-email-header
	
	Question Answers
		'Return-Path'
		'http://www.arin.net/'
		
### Task 5 - Email Body

	Emails can often come with images and attachements. You can get a better look at them by viewing the source code as HTML is what enables these elements to be added to email.
	
	It's important to be careful when interacting emails with attachements as you don't want to accidently open the attachement on your machine as it may be malicious.
	
	It's worth explaing the process to getting the final question
	
	I started by removing the first 4 lines of the email2.txt file and the last line
	I then saved that file and used the terminal to decode the base64 and output the file to a .pdf file. The flag was inside the pdf file
	
	Question Answers
		'https://i.imgur.come/LSWOtDI.png'
		'Payment-updateid.pdf'
		'THM{BENIGN_PDF_ATTACHMENT}'
		
### Task 6 - Types Of Phishing

	Different types of malicious emails can be classified as one of the following:
		Spam - unsolicited junk emails sent out in bulk to a large number of recipients. The more malicious variant of Spam is known as MalSpam.
		Phishing -  emails sent to a target(s) purporting to be from a trusted entity to lure individuals into providing sensitive information. 
		Spear phishing - takes phishing a step further by targeting a specific individual(s) or organization seeking sensitive information.  
		Whaling - is similar to spear phishing, but it's targeted specifically to C-Level high-position individuals (CEO, CFO, etc.), and the objective is the same. 
		Smishing - takes phishing to mobile devices by targeting mobile users with specially crafted text messages. 
		Vishing - is similar to smishing, but instead of using text messages for the social engineering attack, the attacks are based on voice calls. 
		
	Again its worth explaining the final question solution
	I simply went to the provided site about 'defraning' and made the necessary adjustments to the URL. I originally tried it with the =3D=3D however that didn't work so I removed it and that was the solution
		
	Question Answers
		'home depot'
		'support@teckbe.com'
		'Order Placed : Your Order ID OD2321657089291 Placed Successfully'
		'hxxp[://]t[.]teckbe[.]com/p/3DEOowFcEwFHl6EOAyFcoUFV=TVEchwFHlUFOo6lVTTDcATE7oUE7AUET='
		
	