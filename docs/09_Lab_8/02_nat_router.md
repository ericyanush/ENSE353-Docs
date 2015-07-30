The second exercise assigned for the eighth lab was to install and configure services to create a NAT router on our servers.
###Requirements
------------------------

The requirements to create the NAT router are:
1. Functioning Lab Machine
2. [A second network interface.](pci_nic.html)

###Configuring the Second Network Interface
----------------

To configure the second network interface:

1. Launch the Network Manager Text User Interface:
		$ nmtui
2. Highlight the edit connection option with the keyboard arrow keys, and select it with the return key:
	<img src="../img/Lab_8/nmtui.png" alt="Network Manager TUI" class="img-responsive"  style="margin-bottom:20px;height:350px;margin-right:auto;margin-left:auto;">
3. Highlight your new network interface from the list, and select it with the return key. It may have been renamed "Wired Connection 1".
	<img src="../img/Lab_8/nmtui_editconnection.png" alt="Edit Connection Screen" class="img-responsive"  style="margin-bottom:20px;height:350px;margin-right:auto;margin-left:auto;">
4. Highlight the IP4V Configuration option for your connection, and select it using the return key, select Manual from the pop-up list you are presented with:
	<img src="../img/Lab_8/connection_ipv4conf.png" alt="IPv4 Connection Config Type" class="img-responsive"  style="margin-bottom:20px;height:350px;margin-right:auto;margin-left:auto;">
5. Select the show option for the IPv4 configuration.
6. Select the option to "Add" an address to your connection's IPv4 configuration:
	<img src="../img/Lab_8/connection_addaddress.png" alt="Add an address to your connection" class="img-responsive"  style="margin-bottom:20px;height:350px;margin-right:auto;margin-left:auto;">
7. Enter a private range IPv4 address, with a /24 subnet, in my case I chose 192.168.3.250/24:
	<img src="../img/Lab_8/connection_enteraddress.png" alt="Enter address for your connection" class="img-responsive"  style="margin-bottom:20px;height:350px;margin-right:auto;margin-left:auto;">
8. Select the option for your connection's IPv6 configuration, and set it to ignore:
	<img src="../img/Lab_8/connection_ignorev6.png" alt="Disable IPv6 configuration" class="img-responsive"  style="margin-bottom:20px;height:350px;margin-right:auto;margin-left:auto;">
9. Select the `OK` option from the bottom right corner of the dialog to save your configuration changes, and exit the utility.

At this point you will have configured your second network interface to operate your new internal network, however we must configure a few services that will effectively manage the internal network's configuration.

###Configuring Network Administration Services
-----------------

To configure the administration services for the new internal network:

1. Check the configuration of the firewall's public zone:
		$ firewall-cmd --list-all
	<img src="../img/Lab_8/firewall-public-preconf.png" alt="Current firewall configuration for the public zone" class="img-responsive"  style="margin-bottom:20px;height:350px;margin-right:auto;margin-left:auto;">
	Here we see that the new network interface we have installed has been automatically added to the firewall's public zone.
2. Change the zone assigned to your second network interface with the command:
		$ firewall-cmd --change-interface=<your network interface> --zone=internal
	Notice that we did not add the `--permanent` option to the command; if it is added, the command will silently fail. Reload the firewall configuration with:
		$ firewall-cmd --reload
	We can now inspect that the interface has been added to the public zone with the command:
		$ firewall-cmd --list-all --zone=internal
	<img src="../img/Lab_8/firewall-changezone.png" alt="Firewall Change Zones for Interface" class="img-responsive"  style="margin-bottom:20px;height:350px;margin-right:auto;margin-left:auto;">
3. Next we must enable masquerade on the public zone. This allows traffic from the internal network interface to be routed to the interfaces in the public zone (Network Address Translation Routing). We do this with the command:
		$ firewall-cmd --zone=public --add-masquerade --permanent
	<img src="../img/Lab_8/firewall-addmasquerade.png" alt="Add Masquerade to the public zone" class="img-responsive"  style="margin-bottom:20px;height:350px;margin-right:auto;margin-left:auto;">
4. Edit your DNS server configuration, `/etc/named.conf`, to allow it to listen for requests on your internal interface:
	<img src="../img/Lab_8/named_addinterface.png" alt="Allow listening on internal network" class="img-responsive"  style="margin-bottom:20px;height:350px;margin-right:auto;margin-left:auto;">
	Here I have added the address I configured my internal network interface to use, 192.168.3.250.
5. Edit your DNS server configuration, `/etc/named.conf`, to allow the server to perform recursive lookups for clients connecting from the internal interface:
	<img src="../img/Lab_8/named_allowrecursioninternal.png" alt="Allow recursion on local network" class="img-responsive"  style="margin-bottom:20px;height:350px;margin-right:auto;margin-left:auto;">
	Here I have whitelisted recursion for clients that have addresses within the subnet that the address I configured my internal interface is within, 192.168.3.0/24
6. Restart your DNS server to force it to reload its configuration:
		$ systemctl restart named
7. Allow DNS queries through the firewall in the internal zone, and reload the firewall configuration:
		$ firewall-cmd --add-port=53/udp --zone=internal --permanent
		$ firewall-cmd --reload
8. Install a DHCP server:
		$ yum install dhcp
9. Change into the DHCP server's configuration directory:
		$ cd /etc/dhcp
10. Download the preconfigured DHCP server configuration:
		$ curl -O http://cgelowitz.gelowitz.org/dhcpd.conf
	<img src="../img/Lab_8/fetch_dhcpconfig.png" alt="Download DHCP server configuration" class="img-responsive"  style="margin-bottom:20px;height:350px;margin-right:auto;margin-left:auto;">
11. Edit the configuration file you just downloaded. In my case, because I used an address in the 192.168.3.0/24 Subnet, I have only three changes that are necessary, as the configuration file is already setup for a server running a network on that subnet.
	<img src="../img/Lab_8/dhcpconf_hostdns.png" alt="Change the hostname and DNS server adddresses" class="img-responsive"  style="margin-bottom:20px;height:350px;margin-right:auto;margin-left:auto;">
	Here I have changed the hostname of the network to match the hostname that has been assigned to my machine, and I have changed the DNS server address that will be provided to clients connecting to my internal network to the address I have configured for my second (internal) network interface. The changes I have made are circled in red.
	<img src="../img/Lab_8/dhcpconf_defaultgw.png" alt="Change the default gateway" class="img-responsive"  style="margin-bottom:20px;height:350px;margin-right:auto;margin-left:auto;">
	The last change I have to make to this configuration is to the address of the default gateway that is provided to clients connecting to my internal network; here again I have changed the configured address to be the address I configured for my machine's internal network interface. The change is outlined in red.
	
At this point, any single client should be able to connect directly to your machine's internal network interface via a cross-over ethernet cable, or indirectly through a switch. When it does, it will be assigned an IP address by your DHCP server, and will be given enough information (Default Gateway, DNS Server address) that it will be able to connect to the internet by routing traffic through your machine's primary (public) network interface.