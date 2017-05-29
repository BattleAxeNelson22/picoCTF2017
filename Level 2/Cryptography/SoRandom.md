# SoRandom - 75 PTS
We found sorandom.py running at shell2017.picoctf.com:37968. It seems to be outputting the flag but randomizing all the characters first. Is there anyway to get back the original flag?
Update (text only) 16:16 EST 1 Apr Running python 2 (same version as on the server)


## Hint
	How random can computers be?

# Write Up
Download the SoRandom.py.

Then in the terminal nc to the site/prot.

`nc shell2017.picoctf.com 37968`

Running the above command runs the python script we've downloaded and give us the randomized flag:

> Unguessably Randomized Flag: BNZQ:jn0y1313td7975784y0361tp3xou1g44

Lets take a look at the python script:

```python
#!/usr/bin/python -u
import random,string

flag = "FLAG:"+open("flag", "r").read()[:-1]
encflag = ""
random.seed("random")
for c in flag:
  if c.islower():
    #rotate number around alphabet a random amount
    encflag += chr((ord(c)-ord('a')+random.randrange(0,26))%26 + ord('a'))
  elif c.isupper():
    encflag += chr((ord(c)-ord('A')+random.randrange(0,26))%26 + ord('A'))
  elif c.isdigit():
    encflag += chr((ord(c)-ord('0')+random.randrange(0,10))%10 + ord('0'))
  else:
    encflag += c
print "Unguessably Randomized Flag: "+encflag
```

Lets break down the script with some comments

```python
#!/usr/bin/python -u
import random,string
#This is importing 2 different modules, 1st the random module, then the string module. Links about each module are provided in the resources sections.

flag = "FLAG:"+open("flag", "r").read()[:-1]
#this is opening the flag on the end of the server and assigning it to "flag"

encflag = ""
#this is the flag we see, its the new flag after randomization is applied to it.

random.seed("random")
#all randomness needs to be seeded, its where the computer starts thinking when it creates "radomness" true randomness does not actually exist, the computer needs a seed to start at.

for c in flag:
#for every character in flag, do one of the following if statements:
  if c.islower():
    #if the character is a lowercase letter, do the following: encflag adds the randomized character to the encflag value. in order to randomize the letter, this string (ord(c)-ord('a')) turns the letter into unicode and then subtracts 97 from it. 97 is the unicode of 'a'. random.randomrange uses the seed from above to randomly select a number between 0 and 26, then says to modulu 26 it. We then take that value and add it to 97 again, so that the unicode represents a valid lower case letter.
    encflag += chr((ord(c)-ord('a')+random.randrange(0,26))%26 + ord('a'))
  elif c.isupper():
   #if the characters is an uppercase letter, do the following: encflag adds the randomized character to the encflag value. in order to randomize the letter, this string (ord(c)-ord('A')) turns the letter into unicode and then subtracts 65 from it. 65 is the unicode of 'A'. random.randomrange uses the seed from above to randomly select a number between 0 and 26, then says to modulu 26 it. We then take that value and add it to 65 again, so that the unicode represents a valid upper case letter.
    encflag += chr((ord(c)-ord('A')+random.randrange(0,26))%26 + ord('A'))
  elif c.isdigit():
  #if the character is uppercase, do the following, encflag adds the randomized character to the encflag value. in order to randomize the number, this string (ord(c)-ord('0')) turns the number into unicode and then subtracts 48 from it. 48 is the unicode of '0'. random.randomrange uses the seed from above to randomly select a number between 0 and 10, then says to modulu 10 it. We then take that value and add it to 48 again, so that the unicode represents a valid number between 0-9.
    encflag += chr((ord(c)-ord('0')+random.randrange(0,10))%10 + ord('0'))
  else:
  #if the character isn't an uppercase letter, lowercase letter, or number, then it just adds the character to the random flag. This is good for symbols like Underscore or parenthesis. We won't have to decode these since this script isn't randomizing them.
    encflag += c
	
#Now that the script has randomized every letter and number in the flag it prints you the new random character.
print "Unguessably Randomized Flag: "+encflag
```

So the seed is "random" and we need to reverse the encryption, we can do that with the same seed, and reversing the script!

First i just want to explain the formulas a little better with some actual examples to provide a better idea of what they're doing.

So lets use our flag as an example: 
BNZQ:jn0y1313td7975784y0361tp3xou1g44

We know that BNZQ: is going to equal FLAG:

lets put F in as our first c

`B = chr((ord(F)-ord('A')+random.randrange(0,26))%26 + ord('A'))`

So one at a time, 

```python
ord(F)=70
ord(A)=65
70-65=5
#our new formula:
B=5+random.randrange(0,26)%26 + 65
#so what do we need to get B?
ord(B)=66
#subtraact 65 from both sides
1=5+ramdom.randrange(0,26)%26
#so we need 5 and a random number to equal 27 because 27%26=1, therefore our random number must be 22!
```

Now we have to reverse that on every character to get the flag, but keep in mind 22 isn't going to be the same value each time. We can however begin writing the formulas backwards. Lets reverse the above to solve for B. Heres what the formula will look like and an explanaiton.

`F = chr((ord(B)-ord('A')-random.randrange(0,26))%26 + ord('A'))`

Explanation:
```python
ord(B)=66
ord(A)=65
66-65=1
#our new formula:
F=1-random.randrange(0,26)%26 + 65
#so what do we need to get F?
ord(F)=70
#subtraact 65 from both sides
5=1+ramdom.randrange(0,26)%26
#so we need to subtract a random number from 1 and be equal to -21 because -21%26=5, therefore our random number must be 22!
#to confirm my math on that, feel free to insert the following command into your terminal:
python -c "print(-21 % 26)"
#you should get 5.
```

So now that we know the new formula works we need to create a new python script. I named mine: sounrandom.py, and here it is below with comments to explain it.

```python
#!/usr/bin/python -u
import random,string
#This is importing 2 different modules, 1st the random module, then the string module. Links about each module are provided in the resources sections.

flag = ""
#this is our container for where we are going to put each unrandomized character to create our flag.

encflag = "BNZQ:jn0y1313td7975784y0361tp3xou1g44"
#this is the flag we got that we are going to unrandomize

random.seed("random")
#we'll use the same seed to unrandomized, in my example you saw that 22 turned B to F, and also turned F to B when reversed. Because the randomness is 'seeded' we'll get the same random numbers to reverse the encryption that were used to encrypt our flag in the first place.

for c in encflag:
#for every character in encflag, we'll do one of the following if statements:
  if c.islower():
    #if the character is a lowercase letter, do the following: flag += adds the unrandomized character to the flag value. in order to unrandomize the letter, this string (ord(c)-ord('a')) turns the letter from encflag into unicode and then subtracts 97 from it. 97 is the unicode of 'a'. random.randomrange uses the seed from above to randomly select a number between 0 and 26, then says to modulu 26 it. We then take that value and add it to 97 again, so that the unicode represents a valid lower case letter. Remember we are subtracting the random value instead of adding it, to "undo" the "randomness"
    flag += chr((ord(c)-ord('a')-random.randrange(0,26))%26 + ord('a'))
  elif c.isupper():
   #if the character is an uppercase letter, do the following: flag += adds the unrandomized character to the flag value. in order to unrandomize the letter, this string (ord(c)-ord('A')) turns the letter from encflag into unicode and then subtracts 65 from it. 65 is the unicode of 'A'. random.randomrange uses the seed from above to randomly select a number between 0 and 26, then says to modulu 26 it. We then take that value and add it to 65 again, so that the unicode represents a valid lower case letter. Remember we are subtracting the random value instead of adding it, to "undo" the "randomness"
    flag += chr((ord(c)-ord('A')-random.randrange(0,26))%26 + ord('A'))
  elif c.isdigit():
  #if the character is a digit, do the following, flag += adds the unrandomized character to the flag value.. in order to randomize the number, this string (ord(c)-ord('0')) turns the number into unicode and then subtracts 48 from it. 48 is the unicode of '0'. random.randomrange uses the seed from above to randomly select a number between 0 and 10, then says to modulu 10 it. We then take that value and add it to 48 again, so that the unicode represents a valid number between 0-9. Remember we are subtracting the random value instead of adding it, to "undo" the "randomness"
    flag += chr((ord(c)-ord('0')-random.randrange(0,10))%10 + ord('0'))
  else:
  #if the character isn't an uppercase letter, lowercase letter, or number, then it just adds the character to the random flag. This is good for symbols like Underscore or parenthesis. We won't have to decode these since this script isn't randomizing them.
    flag += c
	
#Now that the script has unrandomized every letter and number in the encflag it prints you the new unrandom characters.
print "UnRandomized Flag: "+flag
```

Now run that baby and lets see what we get!

> UnRandomized Flag: FLAG:ac8c0490fb0508767f1625cb8cea8c34

Woohoo! we got our flag!


# Resoruces

https://docs.python.org/2/library/string.html

https://docs.python.org/2/library/random.html#module-random
