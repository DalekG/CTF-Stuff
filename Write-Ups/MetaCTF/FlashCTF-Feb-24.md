# February 2024 FlashCTF

## Web
### Direct Login
!(Write-Ups/Content/MetaCTF-Feb-DirectLogin.png)

Start off by visiting the website link inside of the challenge description.\
At first glance, you will see a normal looking login page.\
!(Write-Ups/Content/MetaCTF-Feb-Web1.png)\
One of the first things that I do when looking at a login page is to right click and inspect the `username` and `password` fields, if there isn't anything interesting in that (which there wasn't anything out of the normal), I will right click and view the page source.\
Scrolling through the page source, I noticed a script that sits at the bottom of the script and notice that it has something to do with the login.\
!(Write-Ups/Content/MetaCTF-Feb-Web2.png)\
In the script, I noticed the line `// Redirect if login successful`\
Looking a little further down I see the directory that the user will be redirected to `./employee_portal.php`\
So I add that directory to the end of the URL to visit the page, and the flag is displayed.\
!(Write-Ups/Content/MetaCTF-Feb-Web3.png)
