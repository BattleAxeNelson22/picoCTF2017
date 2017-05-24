# Little School Bus - 75 PTS
Can you help me find the data in this littleschoolbus.bmp?

## Hint
Look at least significant bit encoding!!

# Write Up
We get to do some steganography, and this hint is going to get us a good starting point: Least Significant bit encoding.

What is a least significant bit? Take a byte 00001001. The least significant bit is the right most bit. thinking outside of bits, take the number 451 for example. The left most number is 4, if we change this number to any other number, our new number could be 100s of numbers away from the original number. However if we change the right most value, 1, our number isn't going to change nearly as much. This is the same concept as least significant bit. So for this challenge we need to read the left most bit of every byte, and reassemble those bits to find the hidden message.

There are a number of tools you can download to help out with this, google searching "Least Significant Bit decoding" will get you a number of them. 

But i prefer stumbling my way through python for a few hours and eventually coming up with something:

```python
# Least Significant Bit script for BMP files.
# Before we start looking at each byte and pulling the least significant bit, we need to ignore the header. To understand a BMP header format take a look at www.fastgraph.com/help/bmp_header_format.html. Based on the information here, the header is 54 bytes.
header = 54

#we are going to want an argument for the flag too, lets create a place to put the flag.
flag = ""

# So lets dump the binary of the bmp file.
with open("littleschoolbus.bmp", "rb") as img:
#with open, opens the file littleschoolbus.bmp, "rb" is the mode we're opening it in, this mode is r-reading b-binary. When we use 'as' we can refer to this string in the rest of the script simply as "img"
	data = bytearray(img.read())
#in the line of code above, we're turning the img file into an array of bytes. bytearray is a function built into python, see the resources below for more information. read() turns the image into a string, instead of a series of numbers, if we dont turn it into a string we might have some problems.

# lets get rid of the pesky header. We're basically saying data now equals data[54] so the next use of data will start at the 55th byte.
data = data[header:]

# Just like the flag, we're going to need a place to dump each byte. After each byte we'll replace this with the next byte.
bits = ""

# Lets start taking a look at each byte. For every byte in the group data we're going to need to pull the last bit. So lets start createing a loop.
for byte in data:
	# Now we need to grab each byte abd pull the least significant bit from each byte and add it to our bis, thats accomplished with the "+=" we want the byte to be read as a string. byte & 1 grabs the last bit of the byte.
	bits += str(byte & 1)
	# Once we have 8 bits in "bits" we need to convert that bit to an ASCII character that we can read. We have to do this before moving onto the next byte, so the subset code will be ignored until bit has 8 bits in it. (if length of bits is 8, do the subest)
	if len(bits) == 8:
		#chr and int are both built in functions on python. before we can turn the 8 bits into a character we need to turn the bits back into a sequence of numbers. Int will do this, so we int the bits, and we do that in base 2, thats going to give us the decimal of the binary. Remember the HASH 101 challenge? Lets say for example that our bits is 11000001. Doing int at base 2 is going to convert that to decimal. decimal of 11000001 is 97. Now chr is a built in python function that can turn that decimal into hex, 97 to hex is 61, chr also turns that hex into a ascii, so 61 in hex is actually the letter 'a' in ASCII, we're doing all that below to get a letter.
		letter = chr(int(bits, 2))
		#Once we have the letter, lets add it the flag.
		flag += letter
		# Once we've got the character added to the flag we need to clear out bits so 8 more bits can be added to it.
		bits = ""
		#This loop will go now until we run out of bytes in data, then it will end so we can print everything we've added to the flag variable:
print(flag)
```

This script, as detailed as the notes are, should get you the flag, along with a whole bunch of gibberish. Hopefully it was easy to understand, if not check out some resources below.

# Resources
https://en.wikipedia.org/wiki/Least_significant_bit
https://docs.python.org/3/library/functions.html
www.fastgraph.com/help/bmp_header_format.html
