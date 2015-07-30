The first exercise assigned for the eighth lab was to install a second network interface into our servers.

###Requirements
------------------------

The requirements to install the second network interface are:
1. Functioning Lab Machine
2. An available PCI slot on Lab Machine's motherboard
3. A PCI Network Interface Card.

###Installing a Network Interface Card.
----------------

To install the new network interface card:

1. Power-off your machine:
		$ shutdown -P now
2. Unplug your lab machine, and open its case.
3. Insert the new network card into any available PCI slot on the motherboard.
4. Plug the machine back in, and boot it.
5. Ensure that the new network card is installed correctly, and is operating correctly:
		$ nmcli d
	There should be a new network interface listed, in my case it is interface enp17s1
	<img src="../img/Lab_8/networkman_interfacelist.png" alt="Network Manager Interface List" class="img-responsive"  style="margin-bottom:20px;height:350px;margin-right:auto;margin-left:auto;">

At this point you have installed a second, fully functioning, network interface into your server. The network interface will be provisioned and configured in the [next lab exercise.](nat_router.html)