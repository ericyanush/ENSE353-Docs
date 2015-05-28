The second exercise assigned for the second lab was to use the ethernet cable built in exercise one to connect your machine to the internet.

###Requirements
------------------------

The requirements to connect to the network are:
1. Completion of all [lab #1](../Lab_1/index.html) components.
2. [A functioning network cable.](Cable_Making.html)

###Getting Online
----------------

To connect the server to the internet the following steps were taken:
1. Ensure you have your functioning network cable connected to both the switch on the bench and the port on the machine.
2. Login to the machine with either the root account or one that has administrative privileges (is in the wheel group)
3. Locate and edit the network configuration script for the ethernet interface. 
	1. The script will be in the directory: /etc/sysconfig/network-scripts. Change the current working directory to here with this command:
			$ cd /etc/sysconfig/network-scripts
	2. As our boxes only have a single ethernet interface we can find the correct file using the autocomplete feature of the BASH shell. Begin by entering the command:
			$ vi ifcfg-enp<tab>
	and pressing the tab key where shown. This will autocomplete the file name to the name of the only matching file in the current directory, in my case ifcfg-enp0s25, and press the return key to open the file the vi editor. Note: if you are not logged into the root user account this command must be prefixed with the sudo (super user do) command.
	3. Move the cursor using the arrow keys to the line in the file that currently reads: ONBOOT=false and press the 'I' key on the keyboard to enable insertion mode.
	4. Modify the line to read ONBOOT=yes
	<img src="../img/Lab_2/enable_interface.png" alt="editing the interface script in vi" class="img-responsive"  style="margin-bottom:20px;height:350px;margin-right:auto;margin-left:auto;">
	5. Exit insertion mode by pressing the ESC key.
	6. Save the changes and quit vi by pressing the following sequence of keys: ':wq<return>'
	7. Reload the network configuration by using the command. Note: The following commands needs to be prefixed with a sudo command if you are not logged in as the root user.
			$ service networking restart
4. Update the system packages to ensure the highest level of network security. This command also requires the sudo prefix if not logged in as root.
		$ yum upgrade
5. Find the current public ip address of the machine by using the command: (It should be of the form: 142.3.xxx.xxx)
		$ ip addr
	<img src="../img/Lab_2/find_ip.png" alt="finding the machine's public ip" class="img-responsive"  style="margin-bottom:20px;height:350px;margin-right:auto;margin-left:auto;">
6. Find the current default route for the machine (the default gateway) by using the command:
		$ ip route
	<img src="../img/Lab_2/find_gw.png" alt="finding the default gateway" class="img-responsive"  style="margin-bottom:20px;height:350px;margin-right:auto;margin-left:auto;">
	
After the completion of the above steps your machine is now connected to the internet, and has a public ip address, which allows it to be directly connected to by any machine connected to the internet.

In this case step 3 and all of its sub-steps are necessary to enable the ethernet interface on boot, rather than having to manually enable it upon every boot. This is something that is new for CENTOs 7 as it has transitioned to having all the network interfaces managed by NetworkManager, in our case however, we do not have network manager installed, and so we must make this change manually.