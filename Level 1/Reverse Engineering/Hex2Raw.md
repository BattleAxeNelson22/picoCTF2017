# Hex2Raw - 20 PTS
This program requires some unprintable characters as input... But how do you print unprintable characters? CLI yourself to /problems/bee57af2b16368039c96edaa1bd95826 and turn that Hex2Raw!

## Hint
Google for easy techniques of getting raw output to command line. In this case, you should be looking for an easy solution.

# Write Up
Change Directory into the problem folder:

`cd /problems/bee57af2b16368039c96edaa1bd95826`

List contents

`ls`

run the hex2raw file

`./hex2raw`
>Give me this in raw form (0x41 -> 'A'):                                                  
>6a5c6fa9602a2d0f439953bcb6f4a611  

Now we know the challenge, we have to convert a string from hex to raw input. 

You could take the hex string to an online conversion tool like this site here: http://string-functions.com/hex-string.aspx to convert the hex to raw, and sure enough you'll get the raw. But you won't be able to pass this in the terminal, it isn't going to understand these characters, so we need to pass the raw text through the terminal in a different way. We are going to want to use the decode modules of python to solve this! Heres the solution and and a break down of what it means.

`python -c "import base64; print('6a5c6fa9602a2d0f439953bcb6f4a611'.decode('hex'))" | /problems/bee57af2b16368039c96edaa1bd95826/hex2raw`

We're going to execute python with -c flag, -c is command so the rest of the input should be read as if its an actual python script. First we want to import the base64 module which includes a decode attribute. The string we have is in hex and we need to decode it into a raw format. So we'll call up the string and use the decode attribute with the ".decode" Once we've decoded the hex, then we can pipe the raw into the hex2raw file.

Congrats, you should be given the flag!

# Resources
http://string-functions.com/hex-string.aspx
