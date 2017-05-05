# Leaf of the Tree - 20 PTS
We found this annoyingly named directory tree starting at /problems/f8fc794974ad619254d983bc423608c6. It would be pretty lame to type out all of those directory names but maybe there is something in there worth finding? And maybe we dont need to type out all those names...? Follow the trunk, using cat and ls!

## Hint
Tab completion is a wonderful, wonderful thing

# Write Up
Change Directory to /problems/f8fc794974ad619254d983bc423608c6, from the terminal.

`cd /problems/f8fc794974ad619254d983bc423608c6`

once here list contents of the directory

`ls'

You'll see "trunk"

The challenge says to follow the trunk, so cd into trunk.

`cd trunk`

once here list the contents of the directory again

`ls`

a branch and a trunk! Given the text in the challenge keep following the trunk, repeat the steps above. Use the hint! for example:

`cd t`

then hit 'tab' to autocomplete and you'll see

'cd trunk47a0'

as you repeat the steps above eventually you'll come to a flag. Once at the flag, display the file by using cat

`cat flag`

congrats you've found the flag!

To make the process of following the trunk easier you don't need to enter each trunk folder, once you hit tab and the folder has autocompleted, just start typing trunk again and tabbing until the autocomplete no longer works. For example:

`cd trunk/trunk47a0/trunk599f/trunk4e66/trunke117/trunk64f5/trunk9721/trunk1e42/'

you could skip the cd even and just do

`cat trunk/trunk47a0/trunk599f/trunk4e66/trunke117/trunk64f5/trunk9721/trunk1e42/flag`

# Resources
nc is a command that runs netcat. Net cat is a Unix utility that reads and writes across network connections using TCP or UDP protocol. 
https://www.sans.org/security-resources/sec560/netcat_cheat_sheet_v1.pdf
