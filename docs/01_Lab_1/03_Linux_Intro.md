With a fully functioning CENTOS install, we were provided with a few linux commands to help us become familiar with Linux and our machines.

We we provided with the commands
	$ dmesg
The dmesg command replays the data in the current kernel log (data collected since last log file rotation).
	$ grep [pattern]
The grep command filters and displays lines of data which contain the data pattern specified
We were also told about the pipe (|), used to _pipe_ the output of one command into the input of another.

We were give the task of identifying the MAC address of the builtin ethernet controller of our machine. The suggested command to identify the address was 
	$ dmesg | grep MAC
However, depending on hardware, in some machines, like mine, this did not identify the MAC address. I found mine by alternatively piping the output of the kernel log into the less command:
	$ dmesg | less
and subsequently reading the file line by line and manually identifying the MAC address. Using this method, it was noted that it would have been more reliable to identify the MAC address of the ethernet device (the only network device on the machine) by searching for the kernel log for the interface name, eth0, using the following command:
	$ dmesg | grep eth0