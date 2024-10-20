Deadface 2024 RP01 Writeups
THESE ARE MY official write ups. You can also look at my reddit but it doesn't include images.
Summary
The Chase
1. Is this Vul-ner-ble
2. Finding The Source
3. Describe Your Perfect Day
4. I Have Layers
5. Base of Operations
6. The Final Showdown
Standalones
1. Hidden in Plain Site
2. Missing Persons
3. Price Check
4. Data Breach
5. Wild Wild West
THE CHASE SERIES
Is This Vul-ner-ble? Crypto
Challenge
Welcome to The Chase challenge series. A series of 6 challenges to hunt and stop DEADFACE as they attack Turbo Tactical.
DEADFACE is at it again! We have started to catch wind of a big attack in the works and we have to stop them before they wreak havoc again this year. This year DEADFACE is going down big time. One of our threat intelligence analysts has been hard at work and thinks they have stumbled onto a breadcrumb trail in GhostTown. Can you follow the trail and ultimately stop DEADFACE?
Submit the flag as `flag{flag}`
Solution
First go the GhostTown and find the chase chatter called 'Yes Its Vulnerable'
Read through and you will find a Burp Suite screenshot with an authorization bearer token and it's posted farther down for easier copy and paste. So, to the trained hacker, they should be able to identify immediately that this is a JWT token. For those not experienced, a quick Google of Authorization bearer token types should find the JWT.
# JWT TOKEN
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCIsIm5vbmNlIjoiIn0.eyJpc3MiOiJ0dXJib3RhY3RpY2FsLm5ldCIsImV4cCI6IjE0NTA2NTkxMDIiLCJ1cG4iOiJjZm9kZXJhIiwiZnVsbF9uYW1lIjoiQ2xhaXIgRm9kZXJhIiwidXNlcm5hbWUiOiJDRm9kZXJhOTEiLCJwaG9uZV9udW1iZXIiOiIiLCJqdGkiOiJmdGlkMjM0MmEtMzI0M2QtMjM0My1kMzR5OHlnZmZlIiwic3R1ZmYiOjExMjIyLCJncm91cHMiOlsibG93X2FkbWluIiwicmVtb3RlX3VzZXIiLCJsYWJ0ZWNoIl0sIm9yZyI6IlR1cmJvVGFjdGljYWwiLCJzdWJfb3JnIjoiR3JvdXBfRCIsIm5idCI6NTc1NDczNzU0ODI5NTI3NTAwMDAsImxkZV9zIjpbeyJhc3RhdHVzIjoibnVsbCIsImJzdGF0dXMiOiJudWxsIiwiY3N0YXR1cyI6InZhbGlkIiwiZHN0YXR1cyI6Im51bGwifV19.kQKRFPLj_SqVeEiBjfKi7FKOEVoV71JgdFRxDTjp7TQ
In the chase chain it talks about how the flag=signature and that its solved with default kali wordlists. This should give them a hint into what they are looking for. Decrypt it for the future challenges.
Then they simply run John with fasttrack.txt to extract the secret.
john jwt.txt -wordlist=/usr/share/wordlists/fasttrack.txt -format=HMAC-SHA256
Finding The Source OSINT
Challenge
We are hot on their feet! In the previous challenge we learned DEADFACE was able to craft their own access tokens. Now we need to determine what they did with this exploit. Use the information learned from the previous challenge and continue to track Deadface in their attack chain?
Submit the flag as `flag{flag_text}`.
Solution
So in challenge 1, we decoded a JWT token, inside this was a username 'cfodera91'.
Use a tool like Sherlock to enumerate other locations where the user has this account.
Doing this you find the that the user has a pastebin. If you visit their pastebin they have several entries but one that leads to a website. [![](https://pastebin.com/favicon.ico)HowIWontheWest - Pastebin.com](https://pastebin.com/WAhPdUf4)
If you follow on to the website, you find a copied version of a website used for phishing. Check the source code for the flag.
flag{the_one_the_only_bad_coding_practices}
Describe Your Perfect Day PCAP
Challenge
From OSINT03, we know the were able to phish some credentials and gain acces into the network. From there, DEADFACE was able to elevate their permissions to the Domain Administrator and steal data. Can you figure out how they could have attacked the network to gain Domain Administrator privileges?
Submit the flag as `flag{flag}`.
Solution
Filter the results for LDAP. Go to packet 1503 and follow the stream. Here you see the output of querying all users in the domain. If you look, you will find a flag inside a users description field unencrypted. SO THIS is an active directory attack where Deadface performed a NULL LDAP Bind to dump all of the users and user information without a credential. In this dump, a users password was left in the Users AD decription which is left in plaintext. This provided them credentials into the network.
I have Layers Crypto
Challenge
During the PCAP02 challenge we capture two important bits of data from Deadface as they conducted their attack against Turbo Tactical. The first bit is that we have this password protected Zip file and no you can't brute force it we have tried. The second bit is this weird jumble of words that does't seem to make much sense. Use the previous challenges resources to figure this problem out, this should not rely on having to guess. Think you can figure out how this relates to the zip file?
SSULKORX WTWAVX GJIWTAY IX KT VTCHCVZS IH YTOL BMSB WTLB IP GM IAJ LOS BMS OBU EOMABCGW NH RIGTIGXFAZSBMWCDDDIWISTXGIBMMMHFTMXIVUBJOHBQN
Solutiuon
In order to figure out the keyword that is used to encrypt the plaintext they will need to use a few clues. The first clue is in the website discovered earlier at [http://3.208.232.204/](http://3.208.232.204/ "http://3.208.232.204/"). If you curl the headers you find a header called X-Secret-Folder which says "misc".
If you navigate to this directory on the website you will find encryption hints at [http://3.208.232.204/misc/](http://3.208.232.204/misc/ "http://3.208.232.204/misc/").
One file talks about backwards vigenere ciphers.
Additionally, there is a word search with words circled including the keyphrase for the encrypted data. Use the decrpytion phrase and decrypt the encryption to find the password.The cipher is a backwards alphabet Vigenere cipher with the keyword
`OnTheNose`
Now decrypt
There was determined to be an issue with how different tools decrypt the password unfortunately since its case sensitive and each tool kicks it out differently.
The deciphered text is:
`Deadface reigns supreme as we continue to tear them down oh by the way the zip password is DoYouReallyThinkYouCanFindMySecretsThatEasily`. Had some syntax issues early with different tools outputting different things.
Inside the zip folder they will see a file called flag.exe but it wont open. If you open the file in a hex editor you will see the first 4 bytes are that of a PNG file.
Change the file back to a png and find the flag.
Base of Operations OSINT
The Challenge
The Turbo team uncovered a lot from that PCAP challenge including the zip file full of images. We also captured a partial BSSID from where the traffic came from "BSSID:10:93:97:". After a review, we think the images are a sort of code to the Deadface members of Lilliths location. Each image has been renamed to what we think the images purpose are in the code. Think you can find the SSID of the completed BSSID?
Submit the flag as `flag{SSID Name}`.

The Solution
First take the image titled "Start Here" and perform a simple Google image search to find that we are in Orlando, Florida.
Now we can begin with any of the images though, some are easier if others are solved first. I recommend the following order.
South

SOUTH IMAGE GIVENa . Reviewing the image we see that we have a food menu with a unique logo. If you focus on the logo you find its called a stubborn mule. If you google restaurants in the Orlando with the word 'mule' or 'stubborn mule' you will find the restaurant.
Finding theb. Review their menu to confirm a match. https://www.thestubbornmuleorlando.com/menu
 
 
2. West
WEST Image Given1. The HTLM is edited to remove the companies name. Pretty easy, Google the License name "License TA298" and find [![](https://ibamusic.com/favicon.ico)IBA Music | An Orlando Based Talent Agency - IBA Music](https://ibamusic.com/) which is located at 1215 E Washington St, Orlando, FL 32801.
Finding the address3. North
NORTH GIVEN IMAGE1. This one is much easier once we have an idea of where our location is. To find this you want to Google Laundry businesses either in the Zipcode or in Thornton Park. This will result in finding the Thornton Park Laundry Facebook page which has the image as its cover picture.East
Laundry mat4. East
EAST GIVENa. Google image search the image to find the family Summerlin.
 
 b. Quick research shows his family sold Lake Eola to Orlando.
c. All Images Found
At this point we have 4 points on a map. If you connect the points, you get a radius for the potential location of the BSSID.
d. Using TheSource image, we can assume its somewhere on E Washington Street.
Now search Washington street and Hill for the partial BSSID
BSSID: 10:93:97:2A:98:60
flag{Treehouse}
The Final Showdown OSINT
The Challenge
The FBI raided `lilith's` location and ended the hacking plan! However, DEADFACE was ready and `lilith`escaped. We got a sneak peak into the building and found a note left behind. Can you track `lilith` to the exact place she is going?
Submit the flag as `flag{flagtext}`. Ensure all letters are lowercase.
The Solution
The note has several key factors to it, the flight times are not perfectly aligned- I didnt adjust this ON PURPOSE, its suppose to be hard:
1. MCO → If you Google this you find it is the Orlando airport.
 
2. Flight 1:
 
 1. 172926300 is Epoch time for October 18th, 2024 at 11:05 AM
 
 2. 2049 is the last 4 of Delta 2049 to New York
 
3. Flight 2:
 
 1. 1729288200 is Epoch for October 18th, 2024 at 5:50 PM
 
 2. 92 is the last part of Delta 92, from New York to Berlin.
This will be a bit of enumeration to discover these flights. The most efficient wy is that once you have the time of the flight and the 2049 or 92, you can look it up at [![](https://www.flightradar24.com/static/favicons/android-icon-96x96.png?t=1728380576)Live Flight Tracker - Real-Time Flight Tracker Map | Flightradar24](https://www.flightradar24.com/) Simply put the country as the US, then Orlando and look at departures and load earlier flights. You can then find the airline flying out to NY. Do the same to find the Berlin flight. These
Now to find the location of the Cafe. If you look at the other clues on the sheet you see Deephax and Coffee? This is a reference to the Vacation chat by Deephax in Ghosttown.
The ChatTHE PLACEIf you do some research on his photos, you will find deep1 shows a place called Distrikt. This is the coffee spot that Lillith intends to meet deephax at.
flag{distriktcoffee}
- - - - - - - - - - - - - - - - NON-THE CHASE PROBLEMS - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Hidden in Plain Site OSINT
Image GivenThe Challenge
We captured a Deadface laptop! During our forensics we discovered a file with a weird tag that said "Hidden Treasure Here". Can you find the treasure's identification code using the image file?
Submit the flag as flag{flagtext}. The flag will begin with a G.
The Solution
1. First run a simple Exiftool on the image to find the creator Wolfpack01.
If you Google this account you will find a lot of different accounts. So this alone isnt helpful.
Now use the image and look really close at the sign that says 4223 RMHC which will lead you to find you are in Colorado, specifically 38°58'13.3"N 104°45'01.2"W.
 
Now you need to figure out what Wolfpack01 hid here.If you look up Wolfpack01 and Colorado you will find a Geocaching account.
GeocachingEach of his geocaches are assigned a custom code that starts with 'G' which should be a giveaway that you are on the right track.
Now its a bit of a game of exploring his caches he has posted. Eventually you will find MonkeyRocks' Cache. If you Google maps the coordinates you will land right where the image is.
Confirming the GeocacheComparing the coordinates of the image "specifically 38°58'13.3"N 104°45'01.2"W." We can conclude this is the right geocahe, the geocache code is GC41VBN.
flag{GC41VBN}
```
Missing Persons OSINT
The Challenge
Your brother, Jake Grahambell a private investigator, was working on a high-profile case when he mysteriously disappeared two weeks ago. The last time you spoke, he mentioned traveling to a remote location and hinted at being in some kind of trouble. There have been rumours spreading in GhostTown about his disapearance as well. Can you piece together the clues and use your OSINT skills to uncover what happened to your brother?
Note: This requires zero attacking or brute forcing. Please do not attack any IP addresses. If credentials are needed, they must be found.
Submit the flag as `flag{flagtext}`.
The Solution

First step is to lookn into Ghost town's to find the chatter Dumping the World? In here are some dumps and you will find one specifically mentioning Jake Grahambell and a link to his X profile.
GhosttownThis is a continuation from last years OSINT challenge Dark Web Dump but that is not a dependency. Users just may notice the same user again. They will search in the bio to find the mention of "Adorable-Welcome-268".
for their Github. This is a two parter: first, go to the users github. Look through the commit history to find the IP address needed.
This is a two parter: first, go to the users github. Look through the commit history to find the IP address needed.
The comment belowSecond using Google or Sherlock you can find this is a Reddit profile.
IMAGE
Now this Reddit section has two parts. First, going through the evidence on the Reddit will disclose a legitimate document for the Dyatlov pass. This will take some effort on their part to find the valid image.
IMAGE
Seen here
IMAGE
Second, in the comments under the post "Survivors Diary" is a TinyURL link to a discord channel open to the public. (The link below has changed to the newer discord link of: [Join the PassingTheSecretsofCorruption Discord Server!](https://discord.gg/BeNAsmmB)
IMAGE
If you join the Discord, you can see a pinned message about the SecretBot. If you talk to the bot and provide it a passphrase of Dyatlov, it will provide you login credentials. NOTE: You can only PM the chatbot and it will provide a hint on how to submit the secret message if you use !
Using the IP address and the credentials you find FTP is open. Login to the FTP server to get the flag.txt. The flag states you must insert the location into the flag to complete it. CREDENTIALS ARE truthseeker:Truth!seeKer123
IMAGE
```
## Price Check STEG
The Challenge
```
Deadface has been hacking random people all over town and no one can seem to figure out what they are doing. All we know at this time is that the most recent site that was hit was Otto's Grocerry Store. Oddly, only the customers are being affected and not the company's network. What is happening? They hired our firm to sniff out the attack and we successfully captured a strange file being sent across the guest WiFi. We believe this is the primary attack vector, and we've heard some victim's mention that they had to "scan" something - maybe it is in the wrong format? Can you figure out how this file is being used in the attack to reveal the flag?
Submit the flag as `flag{flag_text_here}`.
```
The Solution
```
You can make a python code as follows to sort the data back into a QR code.
`import qrcode import numpy as np from PIL import Image solved = np.loadtxt("STEG05.csv", delimiter=",", dtype=np.uint8) solved = solved[::-1] solved[:, [0, -1]] = solved[:, [-1, 0]] Image.fromarray(solved).save("Solved.png")`
The solution QR code
```
## Data Breach PCAP
The Challenge
```
An employee at Turbotactical has been leaking sensitive company information. The issue is we can't quite figure out where the information is getting leaked from. We've captured some network traffic so that you can comb through it and identify where this data is. Can you help us locate the secret flag?
Submit the flag as `flag{flag-text}`.
```
The Solution
```
Simply filter for TCP traffic and follow the packet at NUMBER. Here you see in the header information a custom website header with the flag saved.
flag{Information_disclosure_in_the_head}
```
## Wild Wild West
The Challenge
```
Woah, our network has been ligthing up like the fourth of a July on steriods I tell you HWHAT. Now, given that we had a user call in complaining about weird stuff happening, I think it is about time you cowboy up and figure out what is happening in our here network.
Submit the flag as `flag{flag-text}`.
```
The Solution
```
If you filter through the PCAP you find a significant amount of KRB5 traffic in the network. This should clue them into this being possibly the area of interest. Next you can see traffic such as KRB5KDC Principle Unknown or Preuath required. This should give an indicator that a significant amount of user requests are being made. This requires some knowledge of a Active Directory to understand the kerberos process. Any request with the statement KRB5KDC_ERR_PREAUTH_REQUIRED indicates a valid user that requires pre-authentication.
IMaGE
So I provided 6 valid user accounts in this transaction. Packet 2887 is the target, if you follow the UDP stream you see the flag in the traffic.
IMAGE
flag{kerbrute_finding_users}
```
