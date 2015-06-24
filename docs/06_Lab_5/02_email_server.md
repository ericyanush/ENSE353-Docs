The second exercise assigned for the fifth lab was to install an SMTP server for our new hostname.

###Requirements
------------------------

The requirements to install the SendMail SMTP server are:
1. A functioning Lab machine
2. A functioning [DNS server](dns_server.html)

###Installing SendMail
----------------

To install SendMail on your server:

1. Install the SendMail server:
		$ yum install sendmail-cf m4
2. Edit the configuration `/etc/mail/sendmail.mc` to allow SendMail to listen on interfaces other than the loopback, by changing line 118 to:
		$ dnl DAEMON_OPTIONS(`Port=smtp,Addr=127.0.0.1, Name=MTA`)dnl
3. Recompile the configuration file:
		$ /etc/mail/make
4. Enable SendMail to run for your domain by appending it to the file `/etc/mail/local-host-names`
5. Open port 25 on the firewall:
		$ firewall-cmd --zone=public --add-port=25/tcp --permanent
		$ firewall-cmd --reload
6. Enable the SendMail daemon:
		$ systemctl enable sendmail
7. Start the SendMail daemon:
		$ systemctl start sendmail

At this point you will have a working SMTP server, for the domain name you were assigned. This is of course dependent upon you successfully setting up your DNS server, which serves the DNS MX records that point all other mail servers to your server as the target for mail for your assigned domain. You should at this point be able to send mail to any users that you have created on your server, including root, by sending email to: `<user>@<your.hostname.com>`.
