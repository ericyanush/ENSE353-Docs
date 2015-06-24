The first exercise assigned for the fifth lab was to setup a functioning DNS server using BIND.

###Requirements
------------------------

The requirements to install BIND DNS server are:
1. Functioning Lab Machine
2. Network Connection
3. Assigned hostname (provided by Dr. Gelowitz)
	
###Installing BIND
----------------

To setup BIND, the following steps were taken:
1. Install BIND using yum:
		$ yum install bind bind-utils
2. Start the BIND service daemon:
		$ systemctl start named
3. Enable the BIND daemon to start on boot:
		$ systemctl enable named
4. Open UDP port 53 in the firewall, to allow outside hosts to query our server:
		$ firewallcmd --zone=public --add-port=53/udp --permanent
		$ firewallcmd --reload
5. Install Curl utility:
		$ yum install curl
6. Use curl to retrieve pre-made zone file template
		$ curl -O cgelowitz.gelowitz.org/lab.txt
7. Modify the contents of the file, replacing all references to cgelowitz.gelowitz.org with your assigned domain name.
8. Copy modified zone file to the BIND configuration directory
		$ cp ./lab.txt /var/named/`<your hostname>`
9. Add your new zone to the BIND configuration file by appending a zone block to the end of the `/etc/named.conf` file:
		zone "your.hostname.com" IN {
				type master;
				file "your.hostname.com";
				allow-update { none; };
		};
10. Disable recursion for anyone by localhost, by modifying the lines in `/etc/named.com`:
		//recursion yes;
		allow-recursion { 127.0.0.1; };
11. Enable DNS queries from outside hosts.
	 	listen-on-port 53 { 127.0.0.1; `<Your Public IP>`; };
		allow-query { any ;};
12. Restart the BIND daemon to force reloading the zones and configuration
		$ systemctl restart named

At this point you will have a fully functioning DNS server, that will serves DNS requests for your assigned host name. Note: We took special care to disable recursion for outside hosts, as if it is not disabled, your machine can be used in a DDOS (Distributed Denial Of Service) attack type known as a DNS amplification attack.