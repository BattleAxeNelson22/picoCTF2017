# Digital Camoflage - 50 PTS
We need to gain access to some routers. Let's try and see if we can find the password in the captured network data: data.pcap.


## Hint
It looks like someone logged in with their password earlier. Where would log in data be located in a network capture?
If you think you found the flag, but it doesn't work, consider that the data may be encrypted.

# Write Up
For this challenge you need to download wireshark. https://www.wireshark.org/

Once you have wireshark downloaded, get the data.pcap from the challenge. 

Once you have both wireshark and the data.pcap downloaded, in wireshark go to open, and then browse to the pcap file you downloaded. You should see about 238 lines of data, but the file may vary between users.

We are looking for someones password. Heres the best way to go about searching for it.

The first thing to do is filter the packets down to http. Why? Because if a user submits a username and password on a login screen, the username and password are sent over http, or https(preferabbl).

Filter down to http, simply by typing 'http' in the filter field at the top of the wireshark window.

Now check out a few of the reassembled packets in the 3rd window of your wireshark screen, look for something that appears to call for a password, or "pswrd."

For me this was on my 98th packet, and the packet info was POST /pages/main.html HTTP/1.1 (application/x-www-form-urlencoded).

Once we see the "userid=" and "pswrd=" in the packet details its time to take a deeper look.

In the 2nd frame of the wireshark window, we can see more details about the packet. Digging through it and eventually finding my way to the "HTML Form URL Encoded: application/x-www-form-urlencoded" twisty. Opening that up I see the userid and the password.

> Form item: "userid" = "grassers"

> Form item: "pswrd" = "cHJ2cUJaTnFZdw=="

I recognize the password as base64 encoded, the == is a dead giveaway, when there are emtpy bits in a base64 encoded string, the bits are filled with "=". (the hint also suggested the password would be encoded).

Take the base64 out to a base64 decoder. Thank you google: https://www.base64decode.org/

Enter in the pswrd contents and ensure that UTF-8 character set is selected. Hit decrypt, and congrats, you've found the flag!

# Resources
https://www.base64decode.org/
https://www.wireshark.org/
