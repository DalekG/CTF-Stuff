# February 2024 FlashCTF

## Web
### Direct Login
![Display Web Challenge](../Content/MetaCTF-Feb-DirectLogin.png)\
Start off by visiting the website link inside of the challenge description.\
At first glance, you will see a normal looking login page.\
![Display Web1](../Content/MetaCTF-Feb-Web1.png)\
One of the first things that I do when looking at a login page is to right click and inspect the `username` and `password` fields, if there isn't anything interesting in that (which there wasn't anything out of the normal), I will right click and view the page source.\
Scrolling through the page source, I noticed a script that sits at the bottom of the script and notice that it has something to do with the login.\
![Display Web2](../Content/MetaCTF-Feb-Web2.png)\
In the script, I noticed the line `// Redirect if login successful`\
Looking a little further down I see the directory that the user will be redirected to `./employee_portal.php`\
So I add that directory to the end of the URL to visit the page, and the flag is displayed.\
![Display Web 3](../Content/MetaCTF-Feb-Web3.png)

## Reverse Engineer
### Obfuscated Secrets
![Display RE Chal](../Content/MetaCTF-Feb-ObfuscatedSecrets.png)\
Start off by clicking the link `this Python script`\
When it's downloaded open the script in the text editor of your choice.\
```python
# METACTF FLAG CHECKER

# This program asks the user for a flag and responds with whether
#   the flag they entered is correct or incorrect.

# Receive input
entered_flag = input("Please enter the flag: ")

# Function to validate flags
def validate_flag(flag):
    # Some random string we need. The program doesn't seem to work without it ¯\_(ツ)_/¯
    encrypted = "Lcq]>N?s]bV[R[_OaScQ]]NGPYDKDNG]"
    # Check if length matches
    if len(flag) != len(encrypted): return False
    # Check if entered flag is correct
    return all([ord(flag[i]) - 1 == ord(encrypted[i]) + i for i in range(len(encrypted))])

# Print results
if validate_flag(entered_flag):
    print("Congrats! Your flag is correct.")
else:
    print("Sorry, your flag is incorrect. Please try again.")
```
Notice that there is a variable named `encrypted` that contains what I assumed to be the encrypted flag.\
Knowing that the flag starts with `MetaCTF{` and looking at the `# Check if entered flag is correct line` I was able to assume that the encryption was going 1 letter back for the first letter, 2 letters back for the second, and so on and so on.\
Knowing this, I was able to write a for loop that would handle that encryption.\
```python
def reverse_validate_flag(encrypted):
    decrypted_flag = ''
    for i in range(len(encrypted)):
        decrypted_flag += chr(ord(encrypted[i]) + i + 1)
    return decrypted_flag

encrypted_flag = "Lcq]>N?s]bV[R[_OaScQ]]NGPYDKDNG]"
original_flag = reverse_validate_flag(encrypted_flag)
print("The original flag is:", original_flag)
```
Running this script would reverse the encryption and give us the flag.\
![dispaly flag](../Content/MetaCTF-Feb-ObfSec-Flag.png)

## Binary Exploitation
### Simple Sums
![display chal](../Content/MetaCTF-Feb-SimpleSums.png)\
To start, download the calculator app from the link in the description.\
Review the code in the text editor of your choice.\
```C
#include <stdio.h>
#include <limits.h>
#include <stdlib.h>

void init() {
  setvbuf(stdout, NULL, _IONBF, 0);
  setvbuf(stdin, NULL, _IONBF, 0);
  setvbuf(stderr, NULL, _IONBF, 0);
}

int main() {
  init();

  // Intro text
  printf("Welcome to the SimpleSum calculator!\n\n");
  printf("Provide two integers between 0 and %d, inclusive\n\n", INT_MAX);

  // Get the first integer from user
  int a;
  printf("Enter the first positive integer: ");
  if(scanf("%d", &a) != 1 || a <= 0) {
    printf("Invalid input. Exiting.\n");
    return 1;
  }

  // Get the second integer from user
  int b;
  printf("Enter the second positive integer: ");
  if(scanf("%d", &b) != 1 || b <= 0) {
    printf("Invalid input. Exiting.\n");
    return 1;
  }

  // Add the two integers a and b and print the resulting sum to the user
  int sum = a + b;
  printf("\nThe sum is %u\n\n", sum);

  // Check if the sum equals -1337 and give the flag if it does
  if (sum == -1337) {
    printf("Good job! Here's your flag:\n");
    int ret = system("/bin/cat flag.txt");
  } else {
    printf("No flag for you! Exiting.");
  }
  printf("\n");
  
  // End
  return 0;
}
```
The first thing that stood out to me, was that if I could get the sum to equal `-1337` the app would spit out the flag.\
So I visit the challenge via netcat on the port listed.\
I started off by trying variable a = 2 and variable b = -1339.\
This wasn't allowed since `-` isn't an accepted character.\
Looking closer at the prompt given when the nc connection is made, there is a maximum value that is accepted.\
Since this is Binary Exploitation, and there is a maximum value accepted, there must be a way to trigger an integer overflow.\
Realizing this, I connected via nc again, entered the maximum value accepted, and a negative high integer that would achieve the sum of `-1337`\
Doing this caused an integer overflow and retrieved the flag.\
![display flag](../Content/MetaCTF-Feb-SimpleSumFlag.png)
