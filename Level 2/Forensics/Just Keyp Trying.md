# Just Keyp Trying - 75 PTS
Here's an interesting capture of some data. But what exactly is this data? Take a look: data.pcap

## Hint
Find out what kind of packets these are. What does the info column say in Wireshark/Cloudshark?
What changes between packets? What does that data look like?
Maybe take a look at http://www.usb.org/developers/hidpage/Hut1_12v2.pdf?

# Write Up

Download that pcap, and take a look at it in Wireshark. 

You should see everything is under USB protocol, and the Info says URB_INTERRUPT in.

Not sure what this means, but lets work on the hint: "What changes between packets?"  Clicking through them, in the 2nd window of wireshark, theres a field called "Leftover Capture Data:" that seems to change over time. 

Okay, we know this is a USB device, we know its sending 66 frames of information where very little changes, what could that be? Thinking about the name of this challenge, I'm going to assume its a Keyboard. "Keyp Tyring"

So each of these packet captures reflects a press of a key on a keyboard. We could only assume that what this pcap captured then was someone typingout the flag.

Lets get the meat of the packet, lets get only those payloads that represent the key presses. we can use tshark for that.

`tshark -r data.pcap -T fields -e usb.capdata`

-r[infile] is data.pcap

-T sets the format, and we want fields, the other options won't help us here, we pretty much just want a text file.

-e is used in conjunction with -T we want the fields to be the usb.capdata information, or the payloads.

Once you've run that command on the file, you'll have a list of 60 or so lines that look like this: 

00:00:09:00:00:00:00:00

00:00:00:00:00:00:00:00

00:00:0f:00:00:00:00:00

00:00:00:00:00:00:00:00

And so on..


# Resources
https://www.wireshark.org/docs/man-pages/tshark.html
