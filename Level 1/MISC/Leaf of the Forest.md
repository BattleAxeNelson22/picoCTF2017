# Leaf of the Tree - 20 PTS
We found an even bigger directory tree hiding a flag starting at /problems/db39b5c002d8445dc6d2bbf49a8ccc37. It would be impossible to find the file named flag manually...

## Hint
Is there a search function in Linux? Like if I wanted to 'find' something...

# Write Up
Yes hint, yes indeed there is a way to find something, in fact its called find. get to the directory

`cd /problems/db39b5c002d8445dc6d2bbf49a8ccc37`

instead of listing the contents of directory you could use find, find will list every file every folder here, before you do that use the following command to see how many results you'd get:

`find | wc -l`

when we pipe the results of the find command into a WordCount Lines, it tells us how many directories and files theyre are, the answer is 1,052. Now you could scroll through these and find the path that ends in a flag, or you could use grep.

`find | grep flag`

grep is like 'ctrl-f' it will search the results for the argument you enter, in this case your results should look like this:

`$find | grep flag  
./forest/treeada53a/trunkb393/trunkb8ea/trunka3c4/trunk639d/trunk324e/trunk0bf8/trunkf462
/branchd463/flag`

Yippe, you know where the flag is, now you could cd there and then cat it.

`cd forest/treeada53a/trunkb393/trunkb8ea/trunka3c4/trunk639d/trunk324e/trunk0bf8/trunkf462
/branchd463/`

`cat flag`

And there is your flag for you!

# Resources
http://linuxcommand.org/man_pages/grep1.html
http://linuxcommand.org/man_pages/find1.html
