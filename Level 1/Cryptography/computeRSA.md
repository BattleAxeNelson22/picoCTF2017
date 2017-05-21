# computeRSA - 50 PTS
RSA encryption/decryption is based on a formula that anyone can find and use, as long as they know the values to plug in. Given the encrypted number 150815, d = 1941, and N = 435979, what is the decrypted number?

## Hint
decrypted = (encrypted) ^ d mod N

# Write Up
This one is "simple" math. based on the challenge here's the formula

`flag=(150815) ^ 1941 mod 435979`

We are however going to need a calculator of some kind that can help us solve this. For this challenge, lets again use python.

Good news, you don't even need to boot up a seperate system, we can just use the picoctf terminal.

Python is going to require some different symbols and commands though, heres what we are actually going to use:

`python -c "print((150815 ** 1941) % 435979)"`

First we're telling the terminal to use python, because its powerful. Then we use the -c flag to tell python to run whatever is in the quotes as if its a script. -c specifally stands for "command."

Then we're going to tell python to print the results of our forumla. So insert the forumla into the parenthesis that typically come after the print command.

Double astericks are the same as taking something to a power. After we've taken the encrypted value(150815) and multiplied it to the 1941th power we need to get the modulus. Percent symbol is the python equivialnt of getting the modulus. 

When youre taking the modulus, youre dividing one value by the other, and then rounding that answer down to the nearest even number. so 13 mod 5 is 3, because 13/5 is 2.6. Round that down to 2. so 5x2 =10 the remainder is 3. Therfore 13%5=3. We're doing the same thing in the calcualtion for this challenge but with numbers so large we can't even comprehend them. Thank you python for doing the dirty work.

After entering the command and formula above into the terminal, the terminal will give you the answer.

>133337

That was an easy one!

# Resources
https://en.wikipedia.org/wiki/Modulo_operation
https://docs.python.org/3.0/library/functions.html#print
