#Day1
Not much to write here, just follow the prompts!

#Day 2: Santa's Naughty and Nice Log
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
  
##Question 4: What day was the list stolen?
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
 
  

