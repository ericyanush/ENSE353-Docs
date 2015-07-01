The first exercise assigned for the sixth lab was to setup a functioning POP/IMAP email server using Dovecot.

###Requirements
------------------------

The requirements to install Dovecot server are:
1. Functioning Lab Machine
2. Functioning [DNS server for your domain, and appropriate MX records.](../Lab_5/dns_server.html)
3. Functioning [SMTP server.](../Lab_5/email_server.html)
	
###Installing Dovecot
----------------

To setup Dovecot, the following steps were taken:
1. Install Dovecot
		$ yum install dovecot
2. Edit the Dovecot configuration file `/etc/dovecot/dovecot.conf` to enable the IMAP and POP protocols. You need to uncomment line 24, as pictured below, by removing the octothorp at the beginning of the line.
	<img src="../img/Lab_6/dovecot_config.png" alt="editing dovecot config in nano" class="img-responsive"  style="margin-bottom:20px;height:350px;margin-right:auto;margin-left:auto;">
3. Edit the Dovecot mail configuration file `/etc/dovecot/conf.d/10-mail.conf` to configure the mail storage location for each user. This is done by uncommenting line 25, as pictured below, by removing the octothorp from the beginning of the line.
	<img src="../img/Lab_6/dovecot_location.png" alt="editing the mail location configuration in nano" class="img-responsive"  style="margin-bottom:20px;height:350px;margin-right:auto;margin-left:auto;">
4. Enable the Dovecot server to run on boot:
		$ systemctl enable dovecot
5. Start the Dovecot mail server:
		$ systemctl start dovecot

At this point you will have a functioning IMAP and POP mail server for your domain. Note that no ports were opened in the firewall for the server to listen on, this is unnecessary as we will running a webmail client that will run locally, as opposed to a publicly accessible mail server.