# Leaked Hashes - 90 PTS
Someone got hacked! Check out some service's password hashes that were leaked at hashdump.txt! Do you think they chose strong passwords? We should check... The service is running at shell2017.picoctf.com:4145!


## Hint
See if you can crack any of the login credentials and then connect to the service as one of the users. What's the chance these hashes have actually already been broken by someone else? Are there websites that host those cracked hashes? Connect from the shell with nc.

# Write Up
Check out the hashdump.txt. My file consisted of 51 usernames and their hashed passwords. Per the hint lets check out services that provided already cracked hashes: https://crackstation.net/

Crackstation lets us attempt to crack 20 hashes at a time. I pasted the contents of hashdump.txt into excel, and then dilimited on colon(:) this let me easily grab just 20 hashes.

Paste the hashes into the textbox on crackstation, enter the captcha, and crack those hashes!

Of the 20 I entered, 12 were cracked. The first of which belonged to "myra"

Back in the picoctf console lets connect to the shell:

`nc shell2017.picoctf.com 4145`

I'm prompted for a username, for which I'll use myra.

I'm then prompted for myra's password, and thanks to crackstation, i know the password is c0nus.

Success.

>welcome to shady file server. would you like to access the cat ascii art database? y/n

of course.... yes

Ah some beautiful cat ascii, and a flag!

# Resoruces
