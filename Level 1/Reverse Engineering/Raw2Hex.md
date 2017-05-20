# Hex2Raw - 20 PTS
This program just prints a flag in raw form. All we need to do is convert the output to hex and we have it! CLI yourself to /problems/900be7006255006d8d4e09164dba63c0 and turn that Raw2Hex!

## Hint
Google is always very helpful in these circumstances. In this case, you should be looking for an easy solution.

# Write Up
Change Directory into the problem folder:

`cd /problems/900be7006255006d8d4e09164dba63c0`

List contents

`ls`

run the raw2hex file

`./raw2hex`
>The flag is:10͓͟D█6␉▒├├┌␊▒│␊┼␊┌┼@░␊┌┌↑┬␊␉:/␉┌␊└/9██␉␊7██6255██6␍8␍4␊█9164␍␉▒63␌█$                                               

Well that isn't very helpful!

Now we know the challenge, we have to convert a string from raw to hex. 

Before we start converting everything you see when you run raw2hex, lets cut out the pieces we don't want to convert. We can accomplish this using the cut command. We want to cut out "The Flag is:". Take a look at the manual page for cut(https://linux.die.net/man/1/cut).

So cut lets us print selected parts of lines from each FILE to standard output. We want to pull a field that only contains the flag. By default cut dilimts fields by TAB. There aren't any tabs in the output, but there is simething between the text and the flag itself. The colon(:). We can tell cut to change the dilimiter with the -d flag, by passing : as the argument we'll end up with 2 fields:

First Field: The flag is
Second Field: 10͓͟D█6␉▒├├┌␊▒│␊┼␊┌┼@░␊┌┌↑┬␊␉:/␉┌␊└/9██␉␊7██6255██6␍8␍4␊█9164␍␉▒63␌█$  

Then we just need to tell cut that we only want to grab the second field. We can do that with the -f flag, per the manual page. 

So piping raw2hex to the cut command, dilimitting on colon(:) and then outputting the second field gets us the raw flag.

`./raw2hex | cut -d ':' -f 2
`10͓͟D█6␉▒├├┌␊▒│␊┼␊┌┼@░␊┌┌↑┬␊␉:/␉┌␊└/9██␉␊7██6255██6␍8␍4␊█9164␍␉▒63␌█$ `

We still need to convert that to hex though.

You could probably google something to help you convert it, but your best bet is going to be to just keep solving this problem in the command line. Using the challenges tip to google "convert raw to hex in command line" might eventually get you here: http://stackoverflow.com/questions/6292645/convert-binary-data-to-hex-in-shell-script

Aha, xxd is going to be our answer! take a look at the manual page for xxd. https://linux.die.net/man/1/xxd

You could give it a whirl without any flags:

`./raw2hex | cut -d ':' -f 2 | xxd`

But the output isn't going to help very much. We want it in characters and text that we can read. Searching through the manual page you'll find -p.

>output in postscript continuous hexdump style. Also known as plain hexdump style.

So adding -p will give us a plaintext hexdump of what we pass through xxd.

`./raw2hex | cut -d ':' -f 2 | xxd -p`

Congrats, you should be given the flag!

# Resources
https://linux.die.net/man/1/cut
https://linux.die.net/man/1/xxd
