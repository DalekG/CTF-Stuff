# 2 Boxes 4 Shells
Conduct an nmap scan of 10.8.14.0/24 to find the two boxes ip address\
Box 1 = 10.8.14.13\
Box 2 = 10.8.14.71

## Box 1 User
Conduct an nmap scan of box 1 to see open ports\
Box 1\
21/ftp\
22/ssh\
8000/http

Visit the webpage and notice the different directories open\
"Liam Only!" contains a pdf file that has a vulnerability report\
"Team Effort" contains a list of usernames associated with the team

The vulnerability report shows that ftp login password is easily crackable or is the same as the username\
`ftp 10.8.14.13`\
`Username: lsmith`\
`Password: lsmith`

Move all of the keys from box 1 to your user box\
Change permissions to 600 for all files\
`chmod lsmith* 600`\
`chmod njones* 600`

SSH into box 1:\
`ssh lsmith@10.8.14.13 -i lsmith_rsa`

List directory and cat the token file for box 1 user token

## Box 1 Root
List the root directory and notice that `/root/` is owned by lsmith\
Cd into /root/\
`cd /root/`\
There are two files, the token and a python script\
The token is owned by root so you cannot cat it, but login.py looks interesting\
Cat login.py and see that there are hardcoded username and passwords\
copy and paste those hashes into txt files on the user box\
run `john un.txt/pw.txt --wordlist=/usr/share/wordlists/rockyou.txt --format=Raw-SHA256`\
the hashes will be cracked relatively quick\
go back to the bisonator box and run `su root`\
Password: bison2674\
cat the token file

## Box 2 User
