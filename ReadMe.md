My Challenge Writeups. I made these challenges and wanted to include my writeups.

# PWN07 - Database Crack
We did it, we managed to get a copy of a password database from Johnny Ezell. Can you crack the password to get into the database and see what things lie within?

 Solution
Step 1. 
  The user must take the file and run keepass2john against it:
  CMD: keepass2john mySecret.kdbx > hash.txt  (any name file)
  This will push out a password hash that you need to use JohnTheRipper to crack.

Step 2: 
  Finding the wordlist to crack with is important, Rockyou.txt wont do it. Now if you have an older version of fastrack.txt then it will crack it. If not, you go to ghosttown
  find a wordlist in one of the chat logs. Copy and paste this  into a txt file to make your own wordlist. Here is a small snippet of the words.
          sqlsql
          password1
          password123
          complexpassword
          database
          server
          changeme
          change
          sqlserver2000
          sqlserver2005

Step 3:
  Cracking the hash requires JohnTheRipper and the new wordlist. Simply run the following command. 
  CMD: john --wordlist=<your wordlist> hash.txt
     Note: hash.txt should be the pathway to your hash file from step 1.

Step 3: 
  You have the password to log into the database its complexpassword. Log into the database, open the PWN07 entry to find the flag.

Flag
flag{breaking_the_law}


# OSINT6 - My Next Target
Description
deephax has a new target in mind. Its up to you to discover who it is and warn them before it's too late. He keeps talking about visiting sometime. Use the mySecret.kdbx database from PWN07 - Database Crack to identify the target. The historical data for the database is not related to the challenge and just an artifact.

Step 1: 
Go  to the discussion board and locate a conversation called When I am Not Hacking Im Packing. You find a talk about their favorite leisure places. You will find he mentions a drink called the Lizard Lick Drink from the Caribbean.

Step 2: 
Google "Lizard Lick Drink Caribbean" or "Lizard Lick" carribean bar you will find https://www.lazylizardbarandgrill.com/wp-content/uploads/2019/09/Lazy-Lizard-Menu-2019.pdf

Step 3:You should identify this location as Caye Caulker Belize

Step 4: If you go to Google Images and begin looking at images of this location you will find an image where the user William Jones took a picture at the bar in May 2019. This image will reveal a post where the flag is written. https://www.google.com/maps/@17.7503011,-88.0242681,3a,90y,309.12h,76.72t/data=!3m8!1e1!3m6!1sAF1QipM3jk39GMOAbYhFktOGKYdEGYV3NhagaFOKZQjo!2e10!3e11!6shttps:%2F%2Flh5.googleusercontent.com%2Fp%2FAF1QipM3jk39GMOAbYhFktOGKYdEGYV3NhagaFOKZQjo%3Dw203-h100-k-no-pi0-ya308.2949-ro-0-fo100!7i8704!8i4352
You really need to zoom in on a post that has Ed an Roxie signature on the pole.

Flag
flag{ed_roxie_2019}


#3 PCAP09 - Dreaming of You
Challenge
Someone doesn't understand networking traffic. Can you finds the flag from the PCAP file?
Super simple network review, if you search through the traffic by using the filter "Telnet" you see a whole string of telnet traffic. If you right click and follow the TCP stream
you will be able to scroll through this you will find the flag.


Flag
flag{longing_for_nancy}

# CRYPT009 - Pandoras Box
**Challenge**
Pandora's box, we have found it! Even better, the last travelers knew the numbered code to get in but they couldn't figure out the transcription. Figure out the the transcription's translation to find the flag!

Submit the flag as: `flag{word_word_word_word}`

This is a simple Caesars shift but in a twisted way. The number tells you how far to shift the letter to the right. Simply move the letters over to the left by the numbers to solve the problem.

3686    526   814   518
GUVZ  QGZ  PFV   TVB  
A B C D E F G H I J K L M N O P Q R S T U V W X Y Z

flag{dont_let_her_out}

# STEG08 - Holiday Nesting Doll
Challenge
Holidays are a headache and this one is no different. Mirveal fractured the flag, can you find it? TIME is almost up! Halloween is coming!  
Password: `d34df4c3`

Submit the flag as: `flag{text}`

Solution
Tier 1: Players are given an archive file with three images. They will extract the images from the archive and can run several tools. In order to extract the data from inside, they need to run CMD: steghide extract <IMAGE NAME>. They will be prompted with a password, they will need to crack this with a tool called stegcracker (note this produces a .out file that cant be used to complete the challenge) and the fasttrack.txt wordlist. CMD: stegcracker <IMAGE> <WORDLIST>.  THE WORDLIST IS IN the ghostown thread under More Bitcoin$$$$$$.

Tier 2: They have the Hallowed-Eve, Hallowed-Night, and Hallowed-Spooks images. If you run strings on each image you will find one of the three flag fragments. In order to figure out what to do with these, the description hints they need to be in order and that TIME is almost up. Run exiftool on each image for the metadata to find the XP Comment: Halloween <DATE> <TIME>. Put the flags in order by this time to identify the full flag. 
CMD: exiftool <IMAGE>

CMD: strings <IMAGE> (or any picture)

FInally, of course it is encrypted with base64. A quick cyberchef is needed to find the flag.

SECRET -> If you run steghide on Halloween-Night.jpg with a blank password, you will find a note that hints to look at the meta data.

The full base64: ZmxhZ3tpbmFib3hfaW5hYm94X2luYWJveF9pbmFib3hfaW5hYm94X2luYWJveF9pbmFib3hfaW5hYm94fQ==

Flag
flag{inabox_inabox_inabox_inabox_inabox_inabox_inabox_inabox}

PWN07 - Database Crack
Challenge
We did it, we managed to get a copy of a password database from Johnny Ezell. Can you crack the password to get into the database and see what things lie within?

Submit the flag as `flag{flag text}'

Solution
Step 1.
   The user must take the file and run keepass2john against it:
   CMD: keepass2john mySecret.kdbx > hash.txt  (any name file)
   This will push out a password hash that you need to use JohnTheRipper to crack. The password is found inside fasttrack.txt

Step 2: Cracking the hash requires JohnTheRipper and the fasttrack.txt wordlist. Simply run the following command. 
  CMD: john --wordlist=/usr/share/wordlists/fasttrack.txt hash.txt
  Note: hash.txt should be the pathway to your hash file from step 1.

Step 3: 
   You have the password to log into the database its complexpassword. Log into the database to find the flag.

Flag
flag{breaking_the_law}

PWN08 - Cracking NTLM

Challenge
Vanessa got into our Domain Controller and dumped our hashes and claims she got account credentials! Can you figure out what username and password she may have gotten?

Solution
Step 1. Run impacket against the SAM and SYTEM files to extract the NTLM hashes
  secretsdump.py -sam <SAM FILE> -system <SYSTEM FILE> LOCAL

Step 2: Copy the hashes into a text file and crack them with hashcat using the fasttrack.txt 
  hashcat -m 1000 <NTLM Hashes> <fastrack.txt> --show

Step 3:  You will find the password and need to compare the HASH that comes with the password to identify the account that has been discoverd.
  adminadmin

Flag{itadmin_adminadmin}

