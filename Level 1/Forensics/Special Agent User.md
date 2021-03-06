# Special Agent User - 50 PTS
We can get into the Administrator's computer with a browser exploit. But first, we need to figure out what browser they're using. Perhaps this information is located in a network packet capture we took: data.pcap. Enter the browser and version as "BrowserName BrowserVersion". NOTE: We're just looking for up to 3 levels of subversions for the browser version (ie. Version 1.2.3 for Version 1.2.3.4) and ignore any 0th subversions (ie. 1.2 for 1.2.0)


## Hint
Where can we find information on the browser in networking data? Maybe try reading up on user-agent strings.

# Write Up
For this challenge you need to download wireshark. https://www.wireshark.org/

Once you have wireshark downloaded, get the data.pcap from the challenge. 

Once you have both wireshark and the data.pcap downloaded, in wireshark go to open, and then browse to the pcap file you downloaded. You should see about 238 lines of data, but the file may vary between users.

Lets read up on user-agent strings https://en.wikipedia.org/wiki/User_agent. And how to find them in wireshark: https://www.wireshark.org/docs/dfref/h/http.html

From the link above looks like we can filter on http.user_agent.

I do exactly that with my pcap, and i get 3 packets.

In the 3 packets, i see the 3 user-agent strings. 2 of the user-agent strings are wget, which is not a browser, thats a terminal command. But one of the packets states:

>User-Agent: Mozilla/5.0 (Windows; U; MSIE 9.0; WIndows NT 9.0; en-US))

So i plug "Mozilla/5.0 (Windows; U; MSIE 9.0; WIndows NT 9.0; en-US))" into the flag field and it works!


# Resources
https://www.wireshark.org/docs/dfref/h/http.html

https://en.wikipedia.org/wiki/User_agent
