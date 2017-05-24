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

So how do we translate each of these to a keyboard press? Lets look at the usage tables in the last hint, lets start here: https://docs.mbed.com/docs/ble-hid/en/latest/api/md_doc_HID.html#keyboard

The format of the above payloads, per the link above looks like this:
[modifier, reserved, Key1, Key2, Key3, Key4, Key6, Key7]

So when a key is pressed it fills in the Key1 slot, then when you lift off that key a new event is triggered thats going to be all zeros. Then when you press another key, another event is triggered with the key1 slot having what we're interested in.

If more than 1 key is pressed at any given time we'd see values in Key1 and Key2, and so on if more keys were pressed.

Now lets get rid of everything that doesn't represent a keypress.

`tshark -r data.pcap -T fields -e usb.capdata | grep -v '00:00:00:00:00:00:00:00'`

the -v flag for grep means to grab everything that is not in the quotes, so our new results look like this:

00:00:09:00:00:00:00:00

00:00:0f:00:00:00:00:00

and so on...

So we're most interested in the 3rd field, based on my captures, only 1 key was pressed at any given time. The last 5 fields are all "00"

Lets get a report that only shows that 3rd field then, Key1.

`tshark -r data.pcap -T fields -e usb.capdata | grep -v '00:00:00:00:00:00:00:00' | cut -d ':' -f 3`

cut chops the fields up by the dilimeter -d we're delimiting on colon, then we want only the 3rd field so we -f 3. check out a manual page for cut if you want, we also used cut in some challenges in level 1.

09

0f

04

0a

These are my first 4, but i still don't know what key they represent. Lets take a look at the PDF the hints provided: http://www.usb.org/developers/hidpage/Hut1_12v2.pdf I've done the digging already, jump down to page 53.

We can start translating these identifiers.

09 = f

0f = l

04 = a

0a = g

nice.

Using the rest of the mappings, i eventually get text that looks like my flag:

09 f

0f l

04 a

0a g

00 

2f [

00

13 p

15 r

20 3

22 5

22 5

00

2d -

00

27 0

11 n

1a w

04 a

15 r

07 d

16 s

00 

2d -

00

07 d

08 e
04 a

09 f

06 c

05 b

20 3

1f 2

00

30 }

00 

00 

06 c

or flag{pr355-0nwards-deafcb32}

Now the - should actually be _ when we ignored the first fields we ignored the modifiers which would have actually indicated that shift was also pressed when - was pressed.

New flag:

flag[pr355_0nwards_deafcb32]

Pasted it into pico and aha!!!

Note: if youre fancy you could have used excel or a script or something to translate for you, but i'm not that fancy.




# Resources
https://www.wireshark.org/docs/man-pages/tshark.html
