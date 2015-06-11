The second exercise assigned for the second lab was to install PHPMyAdmin to make administering the LAMP stack easier.

###Requirements
------------------------

The requirements to install PHPMyAdmin are:
1. A functioning Lab machine
2. The completion of all components of [Lab 3](../Lab_3/index.html)

###Installing PHPMyAdmin
----------------

To install PHPMyAdmin on your server:

1. Activate the Extended Packages for Enterprise Linux Repository (EPEL):
		$ yum install epel-release
2. Install PHPMyAdmin
		$ yum install phpmyadmin
3. Edit Apache configuration to allow access to PHPMyAdmin from remote machines:
	a. Move to the working directory:
			$ cd /etc/httpd/conf.d
	b. Make a backup of the config file we are going to edit, so we can rollback any changes, if needed:
			$ cp phpmyadmin.conf phpmyadmin.conf.bak
	c. Insert the following into the line directly after `<Require Any>` appears in the config file:
		`Require all granted`
4. Restart Apache to force apache to reload its modules and configurations
		$ systemctl restart httpd

At this point you will have a working PHPMyAdmin installation, that will be accessible from the outside world.

If you followed my alternative instructions from last week and installed your LAMP stack using docker, you should instead:
1. Get PHPMyAdmin image:
		$ docker pull corbinu/docker-phpmyadmin
2. Start a phpmyadmin container, linking it to your MySQL container. Assuming your MySQL container is named `sql-container`:
		$ docker run -d --link sql-container:mysql -e MYSQL_USERNAME=root --name phpmyadmin -p 3000:80
3. Open port 3000 in the firewall to allow remote access:
		$ firewall-cmd --zone=public --add-port=3000/tcp --permanent
		$ firewall-cmd --reload
At this point you will have a containerized PHPMyAdmin installation, which is accessible from `<your server ip>:3000`. Again containerizing these installs, allows you to run multiple isolated copies of each service, making it easy to separate testing from production or even multiple applications, all running on one server.