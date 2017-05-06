# Leaf of the Tree - 20 PTS
We think someone is trying to transmit a flag over WorldChat. Unfortunately, there are so many other people talking that we can't really keep track of what is going on! Go see if you can find the messenger at shell2017.picoctf.com:61161. Remember to use Ctrl-C to cut the connection if it overwhelms you!

## Hint
There are cool command line tools that can filter out lines with specific keywords in them. Check out 'grep'! You can use the '|' character to put all the output into another process or command (like the grep process)

# Write Up
To get an idea of what youre gonna be working with in the terminal netcat the port.

`nc shell2017.picoctf.com 61161`

You'll notice a lot of text scrolling by, a ton, but we're looking for a flag of some kind, so as per the hint lets use grep again.

`nc shell2017.picoctf.com 61161 | grep flag`

aha! lots of stuff with flag, this isn't going to be as easy as we thought but lets takea  look at the chat for a little bit and see if there is anything we can work with.

>01:23:41 flagperson: this is part 1/8 of the flag - 1a2e

Yes yes, we can work with that, so theres going to be 8 pieces of the flag lets modify the grep so we can find this a little easier

`nc shell2017.picoctf.com 61161 | grep "\/8 of the flag"`

not that i escaped(\) the backslash(/) escaping the backslash will treat it as text, backslashes have a tendancy to mess with commands so lets just treat as text.

give the shell a few seconds to start finding the pieces of the flag, eventually you'll see this:

>01:28:03 flagperson: this is part 1/8 of the flag - 1a2e

>01:28:05 flagperson: this is part 2/8 of the flag - 3d0a

>01:28:07 flagperson: this is part 3/8 of the flag - 6310

>01:28:07 flagperson: this is part 4/8 of the flag - 682c

>01:28:08 flagperson: this is part 5/8 of the flag - 6be0

>01:28:10 flagperson: this is part 6/8 of the flag - e319

>01:28:12 flagperson: this is part 7/8 of the flag - d1b5

>01:28:15 flagperson: this is part 8/8 of the flag - 2f53

Once you have the 8th piece hit Ctrl+C in the terminal, this isn't going to be to copy anything, this is going to get the command to stop working and bring you back to the shell.

You now have your flag! Theres probably some rex or some script we could put together that would actually assemble the flag for you, but for the purposes of this write-up just paste each piece of the string into the flag field and finish it.

# Resources
