# Sunshine CTF 2023

Sunshine CTF Writeup:

## WEB:
### Hotdog Stand:
In the not-so-distant future, robots have taken over the fast-food industry. Infiltrate the robot hotdog stand to find out whatjobs still remain.
https://hotdog.web.2023.sunshinectf.games 

1. Visit URL
2. See if robots.txt is available
	robots.txt contains:
		User-agent: * Disallow: /configs/ Disallow: /backups/ Disallow: /hotdog-database/
3. Visit /hotdog-database/
	prompts a db download
4. Use DB Browser to open the db file
5. Database has table "credentials"
	right-click the table and browse table
	admin login credentials are listed
6. Use login creds to login to the site
	flag is displayed:
				Welcome to the hotdog stand.
		Please take your authentication token: sun{5l1c3d_p1cKl35_4nd_0N10N2}

### BeepBoop Blog:
A few robots got together and started a blog! It's full of posts that make absolutely no sense, but a little birdie told me that one of them left a secret in their drafts. Can you find it?
https://beepboop.web.2023.sunshinectf.games 

1. Visit URL
2. Open Burp and enable proxy
3. Click "View All..." on a post so that the burp proxy catches it
4. Look at the request and the reply
	Notice that there is a hidden attribute on the reply
5. Copy all of the request header from burp
6. Write a script that will run through a loop of the posts (there are 1024 total)
	python has a requests library that will essentially act like curl
```python
		import requests

		requests.packages.urllib3.disable_warnings()

		base_url = "https://beepboop.web.2023.sunshinectf.games/post/"

		for post_id in range(1024):
    		    url = f"{base_url}{post_id}/"
    	  	    headers = {
        		"Host": "beepboop.web.2023.sunshinectf.games",
        		"User-Agent": "Mozilla/5.0 (X11; Linux aarch64; rv:109.0) Gecko/20100101 Firefox/115.0",
        		"Accept": "*/*",
        		"Accept-Language": "en-US,en;q=0.5",
        		"Accept-Encoding": "gzip, deflate",
        		"Referer": "https://beepboop.web.2023.sunshinectf.games/",
        		"Sec-Fetch-Dest": "empty",
        		"Sec-Fetch-Mode": "cors",
        		"Sec-Fetch-Site": "same-origin",
        		"Te": "trailers",
    		}

    		response = requests.get(url, headers=headers, verify=False)

    		if response.status_code == 200:
        		try:
            		    json_data = response.json()
            		    hidden = json_data.get("hidden", False)
            		    print(f"Post {post_id}: hidden: {hidden}")
        		except ValueError:
            		    print(f"Post {post_id}: JSON response not found")
    		else:
        	    print(f"Post {post_id}: Error - Status Code {response.status_code}")
'''
 	    
7. Watch the output to see "Post 608: hidden: True"
8. Use burp to make a request to /post/608
9. Flag will be displayed in the response:
	HTTP/2 200 OK
	Server: nginx/1.18.0 (Ubuntu)
	Date: Sun, 08 Oct 2023 19:35:29 GMT
	Content-Type: application/json
	Content-Length: 66

	{
	"hidden":true,
	"post":"sun{wh00ps_4ll_IDOR}",
	"user":"Robot #000"
	}
	
	
## CRYPTO:
### BeepBoop Cryptography:
Help! My IOT device has gone sentient!

All I wanted to know was the meaning of 42!

It's also waving its arms up and down, and I...

oh no! It's free!

AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
Automated Challenge Instructions

Detected failure in challenge upload. Original author terminated. Please see attached file BeepBoop for your flag... human.

1. Open the file given
	Full of 'Beep Boop'
	Beep = 0
	Boop = 1
2. Convert to binary
	Open file in vim
	:%s/beep/0/g - converts all beeps to 0s
	:%s/boop/1/g - converts all boops to 1s
	:%$/ //g - takes away all spaces
	:%s/\(........\)/\1 /g - adds a space after every 8th character
3. Upload file as input in to CyberChef
4. Use "from binary" recipe
	Notice the output looks similar to a flag:
		fha{rkgrezvangr-rkgrezvangr-rkgrezvangr}
5. Use ROT13 recipe
	Get flag!!
		sun{exterminate-exterminate-exterminate}
