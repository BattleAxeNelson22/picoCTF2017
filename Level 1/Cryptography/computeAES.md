# computeAES - 50 PTS
You found this clue laying around. Can you decrypt it?

## Hint
Try online tools or python

# Write Up
Visit the clue link: https://webshell2017.picoctf.com/static/2e78cf6a104dfa8e0c9220e987aaceb2/clue.txt (yours may vary)

>Encrypted with AES in ECB mode. All values base64 encoded

>ciphertext = rW4q3swEuIOEy8RTIp/DCMdNPtdYopSRXKSLYnX9NQe8z+LMsZ6Mx/x8pwGwofdZ

>key = 6v3TyEgjUcQRnWuIhjdTBA==

The clue suggests using online tools or python. Since i prefer python lets start there.

The rest of this is going to require access to a unix system. I'm using Kali just because. I'm sure that there is a way to use python successefully in a windows system but every time i try that i fail importing modules.

First things first, you may need to install the Crypto module. In the terminal, if youre using Kali like me enter the following:

`sudo pip install pycrypto`

Now its time to write a python script, heres mine with as many comments as necessary to help explain what the script does.

```python
#! /usr/bin/env python 3
# The above line makes this an executable and tells the terminal to use python.
##
# This is a python script for the compute challenge.

from base64 import b64decode

# Import base64de from the base64 module we are going to need to use b64decode. We're going to need this because according to the challenge, all values are base64 encoded. So we're going to need to decode them.

from Crypto.Cipher import AES

# Import AES from the crypto module. This is going to help us use a key to decode AES.

# Before we begin we'll define a variables for each of the things were given. Remember that each of them are base64 encoded, so decode them with b64decode.
key= b64decode("6v3TyEgjUcQRnWuIhjdTBA==")
ciphertext = b64decode("rW4q3swEuIOEy8RTIp/DCMdNPtdYopSRXKSLYnX9NQe8z+LMsZ6Mx/x8pwGwofdZ")

# Now we need to define the mode. There are a handful of encryption modes for AES, for this challenge we're doing ECB mode. Which is Electronic CodeBook. Basically we're using the key to decrypt blocks of text, not each character individually.
keymode = AES.new(key, AES.MODE_ECB)


# Now its time to unencrypt it! We have the key, we have the ciphertext, and we've told the key to decrypt in ECB mode.
flag = keymode.decrypt(ciphertext).decode("utf-8")

#In the above we said, lets use the key in ECB mode to decrypt the cipher text. We want to be able to read it though so we have to convert it to utf-8, which is something we can read. Lastly we need to print the flag. so we can see it.

print(flag)
```



# Resources
https://www.blindseeker.com/AVATAR/
