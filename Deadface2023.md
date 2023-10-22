This is the official write-ups for my challenges for Deadface 2023. I am the develope of these challenges. My focus is to always teach new penetration testing skills in my challenges.

## Steg02 - You've Been Ransomwared
There are several ways this challenge can be solved. First, players can download the image and use their image editor software of choice (e.g., photoshop, GIMP, etc.)Here are two other methods that are free and easy to use:Online-image-editor, Navigate to https://online-image-editor.com and upload the image. Click the Color Change button and the hidden binary will be revealed.Players can use stegsolve (https://github.com/zardus/ctf-tools/blob/master/stegsolve/install ) to rotate through various channels. Green Plane 5 will reveal the binary.
Players can then put the binary into get the message: This ransomware brought to you by mirveal.

## For03 - Malum
A neat issue in an enterprise environment is that if a user accidently types their password into the username input and hits enter, the password will show in plain text in the logs as a failed login. The admin accidently typed their password into the username field and it was registered as a security log.  Look up the log 4625 to find the log with the password.

flag{stabBingStabber1}

## PACP02 - Creepy Crawling
If you filter the traffic by protocol and analyze SSH, you will see a slow but consistent SSH login failure. Reviewing information, you can tell that someone is attempting to brute force their way into the SSH client. At packet 7785 a successful connection is made and the server name is revealed in the info panel.
flag{SSH-2.0-9.29 FlowSsh: Bitvise SSH Server}

## PCAP04 Sometimes IT Lets you Down
This is a basic enumeration of the PCAP file, look through the SMB protocols to packet 8956, you will see the user spooky.domain\mmeyers had performed SMB actions. 
flag{mmeyers}

## PCAP05 - Have a Cup of Coffee
This PCAP teaches of the Active Directory attack, ASREP roasting. If a user has pre-authentication enabled in their user account in Active Directory, a threat actor can obtain the password hashs. If the user tries to access a resource, keberos will send an Authentication Server Request (AS-REQ) message to the domain controller (DC). The timestamp on that message is encrypted with the hash of the user’s password. If the DC can decrypt that timestamp using its own record of the user’s password hash, it will send back an Authentication Server Response (AS-REP) message that contains a Ticket Granting Ticket (TGT) issued by the Key Distribution Center (KDC), which is used for future access requests by the user. You are looking for line: 6828 where you will see KRB5 traffic between the attacking box and the DC. The info will state AS-REP.All the information is here. This gives you the users hash, you will need to put it all together as such: $krb5asrep$<PRINCIPAL_NAME> <FIRST_16_BYTES>$<REMAINING_BYTES>. Completed the hash looks like: $krb5asrep$23$spooky_svc.There are several formats for the flag.
Flag{asrep6828}

## PCAP06 -UVB-76 (Hello,are you there?)
Potentially a lesser known fact but you can hide messages inside ICMP traffic. I wanted to mimick the fun mystery of the UVB-76 mysterious radio broadcast which plays secret messages. You can filter through the traffic and start reviewing each ICMP interaction. You will find early in the packets a hint that says “Keep Looking”. In packets 2625, 2629-2630, you can find the secret message hidden in the ICMP traffic.
flag{is_this_t hing_on?}

## PWN02 - Internal
You must perform OSINT in the Ghostown chats to find the website https://thomascanfield01.wixsite.com/my-site. Once on the website, you can explore links to find that interal_glitterco under the more option in the header tags, there is a download link which gives you the keepass database. Run keepass2john on the file to get the hash. To make life easy, you should identify the password policy in the blog section to find the minimum and max characters for the password. Run CEWL on the website to get a wordlist CEWL <WEBSITE>-m 6 -x 12. Now you will perform a rule based attack on the hash hashcat -a 0 -m 13400 <hashFile> <wordlist> -r .\OneRuleToRuleThemAll. This will find the password of Glitterco! allowing you to login. In the Recycling Bin under “Secret Recipe” you will learn that the URL and password have been seperated and now you know what you are looking for. Explore “Old Access” in internet, click Advanced, the value provides the URL. If you visit the URL, its a password protected file. Go to windows IT_Admin which has the descrtiption OneDrive admin, his password works for the file, boom flag.
flag{!Deadface2023}

## OSINT04 - Dark_Web_Dump
The player will head to the post on Ghost Town called “Dark Web Dumps anyone?” where they will find a Google Drive link that D34th posted. If you run exiftool on the file “LettertoExecs” or “Invited” you find the authors name is Jake Grahambell. You need to search social media to find this user's page on Twitter. The Dear Lucy file gives you a hint to search Twitter for OpticSeltzer69. If you search their tweets you will find a link to their Github page. If you search the files you will find the brown glitter code mentioned in the letter to the execs and hardcoded credentials. The credentials are the flag.
flag{jakeg:MakeitChocolateRain}

## OSINT05 - Murder Mystery
First search branch metals to identify the location, you will be put right into Missouri. This part is a little obscure but they need to do research about the location to figure out that Cement Land is the neighbor to this location. Once they discover that, a little light reading will find the date of death for Bob Cassilly.
flag{SEPTEMBER262011}
‌
## OSINT08 - Take a Seat Upon the Throne
A simple reverse image search can find a similar image of this evil chair of doom identifying it as the Little people village. If you do a little research on the Little People Village in Middlebury, CT you should be able to find that anyone who sits in the chair will die in 7 years! https://www.damnedct.com/little-peoples-village-middlebury/
Flag{middlebury7}


## OSINT09 - Settle in the Presence of Evil
The users will see the highways signs and need to identify where in Connecticut these highways are. A simple search will find that highway 125 is primarily in Cornwall, CT. If you search haunted stories and locations in Cornwall, CT you will find the story of Dudley Town. You must do research on the myths and legends of Dudley Town to learn that Sarah Swift was actually killed by lightning in the 1800’s. http://cornwallhistoricalsociety.blogspot.com/2014/09/the-truth-about-dudleytown.html
Flag{1804lightning}

## OSINT10 - Deadmen are well, Dead.
You need to research famous haunted graveyards in Connecticut and eventually identify Gunntown Cemetary which lists all those buried in the graveyard. In the list is Sabrina Williams. http://www.oxfordpast.com/gunntowncem.html#T
Flag{gunntowncemetary}

## OSINT 11 - The Artifact
There are many ways to discover the meaning but this phrase translates to dabaq or known to us as the Dybbuk. A brief search of dybbuks will educate you on the nefarious dybbuk box.
Flag{dybbukbox}

## PROG05 - Beat the Bank (Broken)
The solution requires you need to deplete the bank of all its money before the 10 second timer resets the values. In order to get the flag however, you need to make 21 calls to the bank within the time frame. 
import socket
import time

Define the IP address and port to connect to

IP_ADDRESS = "143.244.179.162"
PORT = 8000 # Make sure to use the correct port

Number of times to repeat the process

REPEAT_COUNT = 21

Function to interact with the server

def interact_with_server():
    try:
        # Create a socket and connect to the server
        client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        client_socket.connect((IP_ADDRESS, PORT))for i in range(REPEAT_COUNT):
        # Answer the question with "1"
        client_socket.send(b"1\\n")
        response = client_socket.recv(1024)
        print(response.decode())

        # Request "$500"
        client_socket.send(b"500\\n")
        response = client_socket.recv(1024)
        print(response.decode())

        # Sleep briefly before the next iteration
        time.sleep(0)

except Exception as e:
    print("Error:", e)
finally:
    client_socket.close()
if name == "main":
    interact_with_server()
flag{Master_of_Monopoly_Deadface2023}
