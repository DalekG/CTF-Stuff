# Agent Sudo

## Task 1
This task doesn't require any information, just start the box and you're good to go.

## Task 2 - Enumeration
We know the box is up because we just started it. So run your Nmap like this:\
  `nmap -Pn -T4 -p- --open <ip_address>`\

How many open ports?\
3\

How you redirect yourself to a secret page?\
When you visit the port 80, you get the hint:\
  `User your own codename as user-agent to access the site.`\
ANSWER: user-agent

What is the agent name?\
I tried R as the user-agent and got a response with another hint, asking if I was one of the 25 employees.\
This lead me to believe that all the codenames are letters.\
So within the networking tab of web developer tools, I right-clicked and hit edit and resend on the home page.\
I started brute forcing the user-agent, just going through the alphabet. A... B... C... so on.\
Using letter c gave us a new response and page `agent_c_attention.php`\
ANSWER: Chris\

## Task 3 - Hash Cracking and Brute-Force
FTP password\
The webpage from the previous task gives a hint that says the password is weak. Knowing this we can brute force with hydra:\
  `hydra -l chris -P /usr/share/wordlists/rockyou.txt <ip_address> ftp`\
After a few minutes hydra will crack the password.\
ANSWER: crystal\

Zip file password\
After connecting to ftp as chris, there are three files in the directory. Copy all of those over with:\
  `get <filename>`\
Read the contents of To_agentJ.txt\
This gives a hint that the password is inside one of the alien pictures. So check for embedded files:\
  `binwalk cutie.png`\
This will show multiple embedded files, so now extract with binwalk:\
  `binwalk -e cutie.png`\
There is a zip file that will be extracted. To find the password lets use `zip2john`:\
  `zip2john <zip-file> > zip.hash`\
After a few seconds, the password will be displayed.\
ANSWER: alien

steg password\
Start by unziping the zip file now that you have the password.\
  `7z e 8702.zip`\
This will fill up the file To_agentR.txt. Cat that and you will see some kind of encoded string.\
Decode it with:\
  `echo QXJlYTUx | base64 -d`\
This gives us `Area51`\
ANSWER: area51

Who is the other agent (in full name)?
There is another picture file that we pulled down from ftp. So let's run steghide on it to see what comes out.\
  `steghide extract -sf cute-alien.jpg`\
There is a new file named `message.txt`\
When reading this file it gives us the other agents name.\
ANSWER: james

SSH password\
When reading the file mentioned above, you will also recieve the ssh password.\
ANSWER: hackerrules!

## Task 4 - Capture the user flag
For this task, log in to ssh with the username (james) and password received from the last task.\
What is the user flag?\
Once logged in as james, do an `ls -l`, notice the files, `cat user_flag.txt`

What is the incident of the photo called?\
scp the Alien_autospy.jpg back over to your box.\
Do reverse image lookup with the jpg. Go to images.google.com and search with the image.\
This will lead you to an article with the title
