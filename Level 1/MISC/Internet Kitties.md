# Internet Kitties - 10 PTS
I was told there was something at IP shell2017.picoctf.com with port 40660. How do I get there? Do I need a ship for the port?

## Hint
Look at using the netcat (nc) command!
To figure out how to use it, you can run "man nc" or "nc -h" on the shell, or search for it on the interwebz

# Write Up
The hint is pretty straight forward, use netcat. The proper syntax is as follows:

`$nc <address> <port>`

or in this CTF

`$nc shell2017.picoctf.com 40660`

This command will connect to port 40660 at shell2017.picoctf.com, the terminal will be happy and give you the flag.

# Resources
nc is a command that runs netcat. Net cat is a Unix utility that reads and writes across network connections using TCP or UDP protocol. 
https://www.sans.org/security-resources/sec560/netcat_cheat_sheet_v1.pdf
