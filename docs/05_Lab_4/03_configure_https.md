The third exercise assigned for the fourth lab was to configure your machine's webserver to use https.

###Requirements
------------------------

The requirements for configuring HTTPS are:
1. Functioning LAMP stack

###Configuring HTTPS
----------------

To Create a public/private key set and self-signed certificate, the following steps were taken:
1. Install dependancies:
		$ yum install mod_ssl openssh
2. Generate a public/private key pair and a certificate request:
		$ openssl req -new -sha256 -newkey rsa:2048 -nodes -keyout keyfile.key -out request.csr
3. Generate a self-signed certificate:
		$ openssl x509 -req -days 365 -in request.csr -signkey keyfile.key -out certificate.crt

To configure Apache to use HTTPS with the keys and certificates generated above:
1. Copy Keys and Certificate to appropriate directory:
		$ cp certificate.crt /etc/pki/tls/certs/
		$ cp keyfile.key /etc/pki/tls/private/
2. Update the apache config
	a. Open the ssl configuration file for editing:
			$ nano /etc/httpd/conf.d/ssl.conf
	b. Modify the SSLCertificateFile and SSLCertificateKeyFile lines to read
		`SSLCertificateFile "/etc/pki/tls/certs/certificate.crt"`
		`SSLCertificateKeyFile "/etc/pki/tls/private/keyfile.key"`
3. Restart apache to reload configuration
		$ systemctl httpd restart
		
At this point your apache installation will have a function HTTPS system, however browsers will typically warn users that the server's identity cannot be verified, since it is using a self-signed certificate.