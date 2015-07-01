The second exercise assigned for the sixth lab was to install a webmail server for our domain.

###Requirements
------------------------

The requirements to install the SquirrelMail server are:
1. Functioning Lab Machine
2. Functioning [Web server.](../Lab_3/Lighting_The_Lamp.html)
3. Functioning [DNS server for your domain, and appropriate MX records.](../Lab_5/dns_server.html)
4. Functioning [SMTP server.](../Lab_5/email_server.html)
5. Functioning [IMAP/POP Email server.](email_server.html)

###Installing SquirrelMail
----------------

To install SquirrelMail on your server:

1. Install the SquirrelMail server:
		$ yum install squirrelmail
2. Configure the server using the included configuration script
		$ /usr/share/squirrelmail/config/conf.pl
	1. Select option `D` to set predefined settings for specific IMAP servers.
	<img src="../img/Lab_6/squirrel_predefined.png" alt="select predefined options" class="img-responsive"  style="margin-bottom:20px;height:350px;margin-right:auto;margin-left:auto;">
	2. Select the dovecot IMAP server.
	<img src="../img/Lab_6/squirrel_dovecot.png" alt="select dovecot imap server" class="img-responsive"  style="margin-bottom:20px;height:350px;margin-right:auto;margin-left:auto;">
	3.  Save the configuration.
	<img src="../img/Lab_6/squirrel_save.png" alt="save squirrel config" class="img-responsive"  style="margin-bottom:20px;height:350px;margin-right:auto;margin-left:auto;">
	4. Exit the configuration script.
	<img src="../img/Lab_6/squirrel_end.png" alt="exit the configuration script" class="img-responsive"  style="margin-bottom:20px;height:350px;margin-right:auto;margin-left:auto;">
3. Configure SELinux to allow Apache to send mail
		$ setsebool -P httpd_can_sendmail=on
		$ setsebool -P httpd_can_network_connect=on
4. Set mail folder permission to allow SquirrelMail to manage user emails:
		$ chmod 600 /var/mail/*
5. Restart Apache to force reload its configurations:
		$ systemctl restart httpd

At this point you should have a working SquirrelMail installation, and should be able to access your webmail installation at `https://<yourdomain.ursee.org>/webmail`