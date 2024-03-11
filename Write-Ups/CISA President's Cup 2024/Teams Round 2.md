# Teams Round 2

I chose to tackle the `exploitation analyst` challenge for this round and was able to work through and get the flag for all four boxes that needed to be compromised. During the challenge, the team had six hours to work through a series of eight different challenges. I tackled this challenge a little differently than it was laid out by starting with box 2 (unkowingly).lol. My write up will reflect the steps that I took to complete this challenge, but it will not necessarily be in the correct order.

## Box 2
I started this challenge with an Nmap scan of the DMV zone that was given to me in the description.\
At first I ran a simple ping sweep to see what hosts were alive on the DMZ network:\
  `nmap -sn 10.10.10.0/24`\
Unfortunately the hosts were set up to not respond to ping, so I didn't get any hits back. Knowing this I decided to run a scan that would take a little longer:\
  `nmap -Pn -T4 --open 10.10.10.0/24`\
After running this scan I was able to get a response from the `web-server`.\
I visited the web port
