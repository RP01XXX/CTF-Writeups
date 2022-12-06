# Day1
Not much to write here, just follow the prompts!

# Day 2: Santa's Naughty and Nice Log
This room is helpful because logs can assist in most troubleshooting or during an incident. Getting comfortable with reading logs can help progress career.

## Connect to the room
- Putting this here in case you are new and dont know how to do this.
- ssh elfmcblue@<TargetIPAddress>
  - Type "yes" then the password"


## Question 1: How many files are present?
- simply type in ls, no brainer
  
## Question 2: What is the name of the log file?
  - The ls command from above will provide this answer, no brainer
  
## Question 3: What day was sanatas list stolen?
  - Review the log to see what day the logs are from. If you want to hone into this, you can type the following
  - cat webserver.log | grep santaslist.txt   
  
## Question 4: What day was the list stolen?
  - Well the above question should show a date, look it up on a calender.
  
## Question 5: What is the IP
  - Again from your grep command you will have an IP address present.

## Question 6: What was the name of the list that was stolen?
  -  So we kind of already know it but heres how we can find it in two ways.
  - cat webserver.log | grep -e "list.txt"
  - cat webserver.log | grep -e "*.txt"

## Question 7: Look through the log files to find the flag.
  - We know the flag format it THM{} so we can use that
  - cat SSHD.log | grep -e "THM{"
 
  
# Day 3: Nothing Escapes Detective McRed
##Google Dorks
- Google Dorking involves using specialist search terms and advanced search operators to find results that are not usually displayed using regular search terms. You can use them to search specific file types, cached versions of a particular site, websites containing specific text etc.  Bad actors widely use it to locate website configuration files and loopholes left due to bad coding practices. Some of the widely used Google dorks are mentioned below:
- inurl: Searches for a specified text in all indexed URLs. For example, inurl:hacking will fetch all URLs containing the word "hacking".
- filetype: Searches for specified file extensions. For example, filetype:pdf "hacking" will bring all pdf files containing the word "hacking". 
- site: Searches all the indexed URLs for the specified domain. For example, site:tryhackme.com will bring all the indexed URLs from  tryhackme.com.
- cache: Get the latest cached version by the Google search engine. For example, cache:tryhackme.com.


- Questions 1: What is the name of the Registrar for the domain santagift.shop?
- Go to icann.org and search it

- Question 2: Find the website's source code (repository) on github.com and open the file containing sensitive credentials. Can you find the flag?
- Go to github and search the website.

- Question 3: What is the name of the file containing passwords?
-Go through the readme in the github to find this answer

- Question 4: What is the name of the QA server associated with the website?
- Look at the comments in the file

- Question 5: What is the DB_PASSWORD that is being reused between the QA and PROD environments?
- Scroll down for the password

# Day 4: Scanning through the snow

- Question 1: What is the name of the HTTP server running on the remote host?
- Simply run an nmap scan to discover services running on the machine
sudo nmap -sV -p80 X.X.X.X
- We are looking at port 80 because it is the default port for HTTP.

- Question 2: What is the name of the service running on port 22 on the QA server?
- This is simply knowing your ports!

- Question 3: What flag can you find after successfully accessing the Samba service?
- To list to the SMB server: smbclient -L \\\\X.X.X.X -u USERNAME
- To connect smbclient \\\\X.X.X.X\SHARE -u USERNAME
- To get a file type GET FILENAME

- Question 4: What is the password for the username santahr?
- GET userlist
