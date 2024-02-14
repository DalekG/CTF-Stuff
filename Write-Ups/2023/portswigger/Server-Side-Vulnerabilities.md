# Server Side Vulnerabilities

## Path Traversal
Open the challenge and a website named "We Like To Shop" will appear.\
When viewing the source of the page, notice that some of the objeccts are in `products?` and some are in `image?`\
The ones in image will have `image?filename=`, indicating that it is pulling from some file.\
Click the link for any of the images that look like the above.\
Replace `1.jpeg` with `../../../../../../../../../etc/passwd`\
An icon will appear on the screen, save the image to the destination of your choice.\
Open the newly saved jpeg file in a text editor and you will see the contents of /etc/passwd.

## Unprotected Admin Function
After reading the intro before opening the challenge, it talks about `robots.txt` and what it does\
When launching the challenge, the website has a little more functionality than the last\
After the URL, add `/robots.txt`\
Notice that there is a directory that is listed as disallow:\
Add that directory to the end of the URL\
Notice now that you are functioning as an admin and can delete users\
Delete the user `Carlos` listed in the directory, and challenge solved!

## Unprotected Admin Function With Unpredictable URL
Start off by viewing the page source\
Pay attention to the java script that is located after the home href.\
In this javascript there is a line name `adminPanelTag.set href that can be accessed if the user is admin\
Visit that directory, and delete the user Carlos\

## User Role Controlled by Request Parameter
This challenge gives you the directory to go to, `/admin`\
Start by visiting `https://url-here/admin`\
Notice a message that says `Admin interface only available if logged in as an administrator`\
This indicates there is something telling the website that you are or are not an admin\
Since there isn't anything in the url, look at the network tab in web-dev tools and refresh\
Click on the /admin request and look through some of the request headers\
The cookie header has a value of `admin | false`\
If you go to whatever tab cookies are at in your browser, you can double click the value `false` and change it to `true`
Refresh the web page and now you have access\
Delete the user carlos to complete the challenge

##
