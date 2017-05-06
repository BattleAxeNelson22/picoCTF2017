# looooong - 20 PTS
I heard you have some "delusions of grandeur" about your typing speed. How fast can you go at shell2017.picoctf.com:1603?

## Hint
Use the nc command to connect!
I hear python is a good means (among many) to generate the needed input.
It might help to have multiple windows open

# Write Up
In order to do this one, i'd suggest having completed the challenge 'keyz' and be able to write python. The reason i suggest having completed 'keyz' is because this challenge is a lot easier to do in an actual terminal. I'd also suggest having 2 terminal windows open. The following write-up is a walkthrough using 2 terminals on a linux platform.

Authenticate to shell2017.picoctf.com the same way you did at the end of 'keyz' then netcat to shell207.picoctf.com:1603

`$nc <address> <port>`

or in this CTF

`$nc shell2017.picoctf.com 1603`

You'll be greeted with a challenge! This challenge will change everytime you attempt this challenge.

>To prove your skills, you must pass this test.
>Please give me the 'z' character '730' times, followed by a single '6'.
>To make things interesting, you have 30 seconds.
>Input:

The easiest way to do this is going to be to write a very quick python command, here's how to get started. 

`python -c "print 'z'*730 + '6'"`

python tells the terminal we're going to initiate python, the -c flag stands for 'command' this will run the rest of your command string as if its a script, which is why the rest of the command string is in quotes, that the 'script' we're telling python to run. The print command simply prints what you tell it to, in this case we're telling it to print z 730 times and then add the number 6 to the end of that string. Your challenge will be different so replace the variables appropriatly, and you'll be rewarded with the flag.

If youre still too slow you can write and execute a script that connects to the port, parses the information, pulls out the characters and numbers of times they need to be printed, and then returns the requested characters to the terminal. While slightly more challenging is still a good learning expierence, here's a script that will accomplish such task along with comments explaining each element:
```python
`#! /usr/bin/env python 3`
`# The above line makes this an executable and tells the terminal to use python.`
`##`
`# This is a python script for the looooong challenge.`

`import re`

`# Import regular expression, this tells the interpreter that we will use regular expressions in the script. Regular expression will be needed in order to parse the information on the other end of the socket`

`import socket`

`# Import socket module, this tells the interpreter that we're gonna need to do some work with a socket, so it'll import the stuff we need to make this work.`

`# Before we begin we'll define a variable for the socket to make things easier to type out below. After we've defined this variable we can call the module attributes much easier.`
`s=socket.socket()`

`# The first step is going to be to connect to the socket. connect will tell the socket module to connect to the address/port combination provided within the argument, similar to doing nc <address> <port>`
`s.connect(("shell2017.picoctf.com", 30277))`

`# Now we need to read the instructions. recv will tell the socket module to recieve information, we'll need to provide some instructions on how much to recieve, the buffer, and how to interpret the bits that re recieved, to make them legible we want them in UTF8. Define a variable "instructions" as what the socket module reads from the address provided previously.`
`instructions=s.recv(4096).decode("utf-8")`
`print(instructions)`

`# We have the instructions, so now we need to parse them, I'm not going to teach you regular expressions in this script but I'll try to explain what they're doing. `
`# First lets find the letter we need to print a number of times. The following rex will look for the string "'?' character" within the previously defined instructions and define the located character as letter.`
`#the group flag at the end tells you what character in the results to grab, in the 0 column there would be a quote, while the 1st column character is going to tbe the letter we need.`
`letter=re.search("'([A-Za-z])' character", instructions).group(1)`
`print(letter)`

`#now we need to find out how many times we're actually going to print it. same practice as above, but we want the charcter located to be treated as an integer, so when we do the multiplication later the intepreter understands. We'll add a + to the rex, which basically means find as many numbers in this string as possible, we aren't actually looking for a number between 0 and 9, we're looking for multiple digits between 0 and 9. group is gonna be a little funnier here, in the 0 column you'll have the single quote, in the 1 column you'll have the entire integer, whether it be 9 or 999.`
`count=int(re.search("'([0-9]+)' times", instructions).group(1))`
`print(count)`

`#lastly, we need the final character, same theory as above but note that this can be a letter or a number.`
`last=re.search("followed by a single '([A-Za-z0-9])'", instructions).group(1)`
`print(last)`

`#now that we have the variables defined we need to sticth them all together. the "\n" basically tells the console to hit enter.`
`answer=(letter * count) + last + "\n"`

`#now we gotta send them that answer!`
`s.send(answer.encode("utf-8"))`

`#everything above is happening in the background, so we're gonna want to make sure we see the shell's response, which is going to print it to the socket, lets call up the socket again quick and print what it sees, which we really really hope is gonna be the flag.
print(s.recv(4096).decode("utf-8"))`

`#congrats, you got the flag in the most complicated way possible!`
```
That wraps up the script!

# Resources
https://linux.die.net/man/1/python
https://docs.python.org/2/library/re.html#module-re
https://docs.python.org/2/library/socket.html
