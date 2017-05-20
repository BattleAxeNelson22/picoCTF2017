# keyz - 20 PTS
While webshells are nice, it'd be nice to be able to login directly. To do so, please add your own public key to ~/.ssh/authorized_keys, using the webshell. Make sure to copy it correctly! The key is in the ssh banner, displayed when you login remotely with ssh, to shell2017.picoctf.com

## Hint
There are plenty of tutorials out there. This one covers key generation: https://confluence.atlassian.com/bitbucketserver/creating-ssh-keys-776639788.html
Then, use the web shell to copy/paste it, and use the appropriate tool to ssh to the server using your key

# Write Up
For this challenge you could follow the tutorials within the hints, but I'll provide alternative guidnace to completing this challenge.

To access the challenges remotely and using SSH keys on your windows system, install the following programs:

Puttygen(http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
mRemoteNG(http://www.mRemoteNG.org/)

We're going to use PuTTYgen to generate a pair of SSH keys, and we'll use mRemoteNG to remotely access picoCTF.

**Generate a Key:**

Open puttygen.exe, after installing puttygen. In the Key menu make sure "SSH-2RSA key" is checked, and the radio icon for RSA under Parameters is selected. You can leave the bits for the key at 2048, but i'd recommend increasing the key size to 4096.
Now click 'Generate' and move your mouse around the blank space as the program instructs. Moving the mouse around creates randomness that helps generate a unique key.

Once done you'll have your key pairs. Change the key comment to PICOCTFSSHKEY, you could change it to whatever you want, or leave it, but whenever you create a key pairing i recommend putting a comment here that helps you remember what the keys are for. Leave the passphrases blank for this CTF exercise so that you don't have to enter in password everytime you SSH to the challenges.

Choose a safe location to store your keys, ideally, create a directory somewhere specifically for this CTF and create a keys folder in there. Save both the public and private keys to that location. Save the public key as "PICOCTF.pub" and save the private key as "PICOCTF.ppk".

Congratualations, you just created a secure key pairing!

Now, as per the challenge, we need to put the public key in ~/.ssh/authorized_keys, before we do that we need to make some modifications to the public key file.

**Edit Public Key:**

Public keys are expected to be in a very specific format. When PuTTYgen created our public key, it wasn't in a format that PICOCTF is going to expect, so lets change it.

Open the PICOCTF.pub in a text editor. I highly recommend Notepad++(https://notepad-plus-plus.org)

Once you have the public key open in your text editor it will look something like this:

>---- BEGIN SSH2 PUBLIC KEY ----
>Comment: "PICOCTFSSHKEY"
>AAAAB3NzaC1yc2EAAAABJQAAAgEAhOWOIDVpdMLs6M1rw7tAg2YLfaDYin6nhzDa
>bJpPDUHwDRRsQvgzxaSf7j192f/DbPlv1WySKUrycEbhtUCCi+ucd3fNuLhiGdXa
>1igqsSklqWSVdIMmxUfZjQuMmHfsNo69GLSt6Zt1uY9SPhymOl7EILbt4J9ow1Yw
>mPORcihdjSqY5elJg1ryxRik8JYbDvL2MS/2Kjc+PGqJoZOAPiNzK8E1iEEdFNdF
>BoH+nxfeN5miYYNL7arOI9FkB2OUSsUgJcIr0xd6zO3mzVMxiNtg0vjxmceAFFxZ
>n7u7xyiTPd8+UKEeitacfuuDBGyCzMz+i4x+oAbPMaLMi0goo6qn5ZrBxD4IV8A8
>IDfnQR5beWw9aGaF+xwfnwldP2UTrKzVRlifN+rme/GF3x1ktc1GAYIVU35TpcCX
>Fj7p9QQcHgs6SJ4OZTtU8qiFhXiWQHRXathCIDs8oDB9jCz8RFO2/5ZTDiC0Zyg9
>ry9w9mntNnASM/F28C5wedUJ5chuA9/SBRmf9UJ7Qu5O+wnk3agOY5V8ooCGzFAz
>ouqH5otnd0m24699rSWGhFPTA9jkp1URA8p9NeYdy73MJ7C7sbKmmLPdskyetk8u
>KqcgAWepXjp0oJ0oYZZ/yWuazQvKOUQHqQfOU5NS1WT58aH452u/NOFj1o7WxFA3
>RtvWjcc=
>---- END SSH2 PUBLIC KEY ----

Thats going to be a problem so delete the following lines:

>---- BEGIN SSH2 PUBLIC KEY ----
>Comment: "PICOCTFSSHKEY"
>---- END SSH2 PUBLIC KEY ----

Then for the remaining text, we want it to all actually be on one continuous line. The easist way to do this is going to to use find and replace in Notepad++. If you go to manually delete the line breaks, you'll actually delete the first character of the next line too, so don't try to do that or your key pairings will never work.

In Notepad++ select all of your remaining text and then click search and then replace in the dropdown menu. In the Find what: field enter "\r\n" without the quotes. Then uncheck the boxes for "Match whole word only", "Match case", and "Wrap around". Lastly ensure the radio for Extended (\n, \r....) is selected.

\r\n means CarriageReturn(\r) and LineFeed(\n) these are used to create new lines in certain windows documents. We're replacing them to put the entire key string on one line.

If you have to scroll pretty far to the right to now see your entire key string i'd recommend turing on "Wrap" in Notepad++(View > Word Wrap).

We aren't done editing the key text yet, theres a few things we need to add. At the beginning of the key add "ssh-rsa " (include the space). This tells the system we're using an RSA SSH key. at the end of the key add " PICOCTFKEY"(include the leading space) this is your key comment. This will still be used to help you identify the key. Now save the new file as PICOCTF-formatted.pub to the same directory as your raw public and private keys.

**Provide your key:**

Close out of Puttygen, close out of notepad++ and now open mRemonteNG.

In your Connections Window, highlight "Connections" and then click the folder with the plus to create a new Connections folder. Name your new folder PICOCTF, and then highlight the folder. Now click the new connection icon, to the left of the new folder icon.

In the config window make the following updates:
Name: picoctf
Hostname/IP: shell2017.picoctf.com
Username: <use the username you registered with pico>
Password: <your password for picoctf>
Protocol: SSH version 2
Port: 22
Putty Session: Default Settings

Now you should be able to double click your connection in the Connections tab, and you will be automatically connected to the picoctf system.

Now in mRemoteNG, click on Tools > Options > Advanced. Then in the advanced menu click on Launch PuTTY. This will open a window called PuTTYNG Configuration. In the menu on the left expand(+) SSH and then double click on Auth. Once on this menu click the "Browse". Browse to your PRIVATE key, this is the one with the .ppk extension. Once its selected and you are back on the PuTTYNG config menu with the key path in the Browse box, then click on Session. Under "Saved Sessions" in the texbox enter "PICOCTF_SSH", and then click Save. This will return you to mRemoteMG

In the COnfig window change the follwoing field:
Putty Session: PICOCTF_SSH

You should still have a terminal open in mRemoteNG for picoctf, if not start up a new connection by double clicking picoctf in your Connections menu. (You'll see your key was refused, but you'll still be connected via Username and Password).

Now we need to put our public key on the system. First you'll need to create the directory, and the authorized key file.

`mkdir ~/.ssh;chmod 700 ~/.ssh; touch ~/.ssh/authorized_keys;chmod 600 ~/.ssh/authorized_keys`

We did 4 different commands in that one command string, each command was seperated by a semi colon. Heres a breif explanation of what each command did:
mkdir ~/.ssh - This created a directory titled .ssh
chmod 700 ~/.ssh - This changed the rights on the .ssh direcotry. 700 allows the owner(you) to read, write and execute the directory.
touch ~/.ssh/authorized_keys - Touch, while you can do a few things with it, in this case creates a new file inside the .ssh directory called "authorized_keys" because the file did not already exist.
chmod 600 ~/.ssh/authorized_keys - This changed the rights on the authorized_keys file. This allows the owner(you) to read and write to the file.

now to verify you created the file and folder run:

`ls -al .ssh`

This command will show you all files within the .ssh directory. You should see the authorized_keys file listed along with the permisions. the first three characters should be -rw. The first 3 characters are the permissions of the owner, r means read, and w menas right. The rest of the fields are - which means no one else has permissions to these files.

The last thing we need to do is put our public key into the authorized_keys file.

Go open the PICOCTF-formatted.pub file in a text editor(notepad++) and copy the entire contents of the file. Back in the command line on mRemoteNG do the following:

`echo '<rightclick>' >> .ssh/authorized_keys`

Your command should look similar to this:
`echo 'ssh-rsa AAAAB3NzaC1yc2EAAAABJQAAAgEAhOWOIDVpdMLs6M1rw7tAg2YLfaDYin6nhzDabJpPDUHwDRRsQvgzxaSf7j192f/DbPlv1WySKUrycEbhtUCCi+ucd3fNuLhiGdXa1igqsSklqWSVdIMmxUfZjQuMmHfsNo69GLSt6Zt1uY9SPhymOl7EILbt4J9ow1YwmPORcihdjSqY5elJg1ryxRik8JYbDvL2MS/2Kjc+PGqJoZOAPiNzK8E1iEEdFNdFBoH+nxfeN5miYYNL7arOI9FkB2OUSsUgJcIr0xd6zO3mzVMxiNtg0vjxmceAFFxZn7u7xyiTPd8+UKEeitacfuuDBGyCzMz+i4x+oAbPMaLMi0goo6qn5ZrBxD4IV8A8IDfnQR5beWw9aGaF+xwfnwldP2UTrKzVRlifN+rme/GF3x1ktc1GAYIVU35TpcCXFj7p9QQcHgs6SJ4OZTtU8qiFhXiWQHRXathCIDs8oDB9jCz8RFO2/5ZTDiC0Zyg9ry9w9mntNnASM/F28C5wedUJ5chuA9/SBRmf9UJ7Qu5O+wnk3agOY5V8ooCGzFAzouqH5otnd0m24699rSWGhFPTA9jkp1URA8p9NeYdy73MJ7C7sbKmmLPdskyetk8uKqcgAWepXjp0oJ0oYZZ/yWuazQvKOUQHqQfOU5NS1WT58aH452u/NOFj1o7WxFA3RtvWjcc= PICOCTF' >> .ssh/authorized_keys`
 
Then hit enter. To verify the key information was sent to the authorized key file you can cat it.

`cat .ssh/authorized_keys`

And you should see the contents of the file look like your formatted key file.

We have one last thing to do.

Connect to picoctf using SSH keys:
in mRemoteNG, close your connection. In the Config menu for picoctf delete your password from the password field.

now in the conenctions window double click your picoctf connection, to connect.

If you did everything correctly you should see the following:
>Congratulations on setting up SSH key authentication! 
>here is your flag: who_needs_pwords_anyways

Congrats, hopefully you learned a little something here and got your flag. Now you can do the picoctf challenges all inside mRemoteNG! You will still need to access the challenges on the web page, and you'll still need to enter each flag there, but I prefer the terminal inside of mRemoteNG over the terminal inside of the webpage anyday.
 
# Resources
https://www.blindseeker.com/AVATAR/
