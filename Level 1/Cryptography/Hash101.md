# Hash101 - 50 PTS
Prove your knowledge of hashes and claim a flag as your prize! Connect to the service at shell2017.picoctf.com:46122


## Hint
All concepts required to complete this challenge, including simple modular math, are quickly found by googling :)

# Write Up

Netcat to the url and port:

`nc shel2017.picoctf.com 46122`

And you'll see the challenge.

>Welcome to Hashes 101!
>
>There are 4 Levels. Complete all and receive a prize!
>
>
>**-------- LEVEL 1: Text = just 1's and 0's --------**
>All text can be represented by numbers. To see how different letters translate to numbers, go to http://www.asciitable.com/
>
>TO UNLOCK NEXT LEVEL, give me the ASCII representation of 0111000001101100011000010110100101100100

Per the hint we can just google a lot of this. So look for a Binary to ASCII converter. I prefer: http://www.rapidtables.com/convert/number/binary-to-ascii.htm

After entering in the binary from the command line and hitting convert you should get a single word. I did the challenge a few times and the answer will vary. So type the answer you get into the command line.

> \>pwning

Your answer should be accepted and you'll be provided the next step of the challenge:

>Correct! Completed level 1
>
>**------ LEVEL 2: Numbers can be base ANYTHING -----**
>Numbers can be represented many ways. A popular way to represent computer data is in base 16 or 'hex' since it lines up with bytes very well (2 hex characters = 8 binary bits). Other formats include base64, binary, and just regular base10 (decimal)! In a way, that ascii chart represents a system where all text can be seen as "base128" (not including the Extended ASCII codes)
>
>TO UNLOCK NEXT LEVEL, give me the text you just decoded, pwning, as its hex equivalent, and then the decimal equivalent of that hex number ("foo" -> 666f6f -> 6713199)

Note that the word you may need to decode could be different, for this example, my word is "pwning."

Look for an ASCII to Hex converter via google, i prefer: http://www.rapidtables.com/convert/number/ascii-to-hex.htm

After entering in the word "pwning" and hitting convert i get the hex "70 77 6e 69 6e 67"

Enter it into the command line without the spaces, and if its accepted you'll be asked for the decimal.

>Now decimal
>dec\>

This time look for a converter to get you from Hex to Decimal, again i prefer rapid tables: http://www.rapidtables.com/convert/number/hex-to-decimal.htm

Enter in the hex without the spaces and you'll get the decimal value for pwning the decimal value was "123658255822439"

Enter your decimal into the terminal, and move onto step 3!

>Correct! Completed level 2
>
>**----------- LEVEL 3: Hashing Function ------------**
>A Hashing Function intakes any data of any size and irreversibly transforms it to a fixed length number. For example, a simple Hashing Function could be to add up the sum of all the values of all the bytes in the data and get the remainder after dividing by 16 (modulus 16)
>
>TO UNLOCK NEXT LEVEL, give me a string that will result in a 9 after being transformed with the mentioned example hashing function

This one is pretty hard to explain, heres some sample responses:

a - sum of all characters = 97. 97 mod 16 = 1. 

b - sum of all characters = 98. 98 mod 16 = 2.

c - sum of all characters = 99. 99 mod 16 = 3.

d - sum of all characters = 100. 100 mod 16 = 4.

That should give you idea of wher we're going with this. But how do you know what the sum of a binary is going to be? How is a = 97?

1st, use an ascii to hex convert to convert "a" to hex. And you get 61. Then use a hex to decimall converter to convert "61" to decimal, and you get 97.

modulus(mod) means to get the remainder of a division. so 97/16 = 6.0625, but we want to round that down to 6. Then take 16x6 to get 96. Then subtract this from the original numbre, (97) so 97 mod 16 = 1.

Its a little complicated sure. 

So for this level, use the appropriate letter to get you the correct modulus. Since my challenge is to get 9 as a result, i'll use the 9th letter of the alphabet "i".

>Correct! Completed level 3
>
>--------------- LEVEL 4: Real Hash ---------------
>A real Hashing Function is used for many things. This can include checking to ensure a file has not been changed (its hash value would change if any part of it is changed). An important use of hashes is for storing passwords because a Hashing Function cannot be reversed to find the initial data. Therefore if someone steals the hashes, they must try many different inputs to see if they can "crack" it to find what password yields the same hash. Normally, this is too much work (if the password is long enough). But many times, people's passwords are easy to guess... Brute forcing this hash yourself is not a good idea, but there is a strong possibility that, if the password is weak, this hash has been cracked by someone before. Try looking for websites that have stored already cracked hashes.
>
>TO CLAIM YOUR PRIZE, give me the string password that will result in this MD5 hash (MD5, like most hashes, are represented as hex digits):
>96e68796a3b073205bb8d5688ded0934

We're going to need to look for a database of already cracked hashes. Off to google, but if youre lazy head on over to https://crackstation.net/.

Lots of people all over the world in the history of hashes have been cracking them. Crackstation is one of the sites that compiles tons and tons of successfully cracked hashes, like the one you'll be given in this challenge. Pass the MD5 hash into the input field on crackstation and let it search its tables for the hash. It should find it, and if so will give you the Result in Green. For the hash in my challenge the result was "h4ugh."

Enter the result in the command line as the password.

>Correct! Completed level 4
>You completed all 4 levels! Here is your prize: a24c028f0a572b9101afd00c86734472

Hooray! Now enter your flag, and youre done here. If you aren't familiar with anything that just occured check out the resources below.

# Resources

The resources below would help you manually do the conversions we did in these challenges:

http://stackoverflow.com/questions/6826516/how-exactly-does-binary-code-get-converted-into-letters

http://ascii.cl/

http://www.permadi.com/tutorial/numHexToDec/
