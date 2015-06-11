The first exercise assigned for the second lab was to get NTP (Network Time Protocol Functioning).

###Requirements
------------------------

The requirements to install NTP are:
1. Functioning Lab Machine
2. Network Connection
	
###Installing NTP
----------------

To setup NTP, the following steps were taken:
1. Install NTP using yum:
		$ yum install ntp
2. Start the NTP service daemon:
		$ systemctl start ntpd
3. Synchronize the time to the current network time:
		$ ntpdate timelord.uregina.ca
4. Enable the NTP daemon to run on system boot:
		$ systemctl enable ntpd

At this point your server will have the NTP client running, and your server's time will have been synchronized with the network time server.