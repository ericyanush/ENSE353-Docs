## Lab 3 - Lighting the LAMP
-----
The goal, and the only exercise for the third lab is to setup a LAMP stack on our servers.

###Requirements
------------------------

The requirements for setting up a LAMP stack are:
1. Completing all of the previous lab exercises.
	
###Installing LAMP
----------------

To Install and configure the LAMP stack
1. Install Apache Web Server, MySQL database (MariaDB) and PHP using the following command
		$ yum install httpd mariadb-server mariadb php php-mysql
2. Install a better text editor than Vi, and emacs for Dr. Gelowitz with the command:
		$ yum install nano emacs
3. Start Apache, and enable the Apache service to start on boot using these commands:
		$ systemctl start httpd
		$ systemctl enable httpd
4. Start mariadb and enable the mariadb service to start on boot using these commands:
		$ systemctl start maraidb
		$ systemctl enable mariadb
5. Secure your new mariadb installation by running the command:
		$ mysql_secure_installation
	When prompted, setup a root password, remove anonymous users, and disable remote access.
4. Allow http traffic through the firewall by adding port 80, using these commands:
		$ firewall-cmd --zone=public --add-port=80/tcp --permanent
		$ firewall-cmd --reload

At this point you now have a functioning LAMP stack, that will serve webpages out of the /var/www/html/ directory.
Although this is sufficient to setup a basic LAMP stack, I chose to install the AMP (Apache, Mysql, PHP) portion of the stack in an alternative way, using linux containers and Docker.


### Installing AMP in Docker
--------------------------------------

Installing the Apache/Mysql/PHP portion of the stack has many advantages, including the ability to run several copies of the stack at one time on a single machine. Containerization of the stack also adds another level of security between the big bad internet and your bare services, and also isolate  them from your bare server and from other containers running on your server.

To install and configure AMP in Docker:
1. Install Docker using the command:
		$ yum install docker
2. Pull the MySQL container image from the docker registry:
		$ docker pull mysql
3. Start a MySQL container to be used for our new PHP application:
		$ docker run --name mysql-lab3 -e MYSQL_ROOT_PASSWORD `<some root password>` -d mysql:latest
4. Create a new temporary container to use to login into the mysql container. We need to use another container to do this as by default we have not exposed port 3306 from our container, so we cannot connect directly to the mysql instance, so instead we will create a temporary container to which we will link port 3306 from our mysql instance so that we can gain temporary access to the DB to setup our databases and tables.
		$ docker run -it --rm --link mysql-lab3:mysql-lab3 mysql sh -c 'exec mysql -h "$MYSQL_PORT_3306_TCP_ADDR" -P "$MYSQL_PORT_3306_TCP_PORT" -u root -p"$MYSQL_ENV_MYSQL_ROOT_PASSWORD"'
	This command starts the temporary container and links it to the mysql container we created in step 3. The --rm switch marks this temporary container for automatic deletion as soon as we exit it. The container is started, and automatically launches a mysql interactive terminal session for our mysql server.
5. Configure databases and tables in the server as needed, typing `quit;` at the prompt when finished.
6. Pull a docker image containing PHP and Apache from the docker registry:
		$ docker pull eboraas/apache-php
7. Create a directory on the server from which the new PHP application will be served from:
		$ mkdir -p /var/apps/lab3
8. Modify the SELinux policy for the directory to enable the docker container to access it:
		$ chcon -Rt svirt_sandbox_file_t /var/apps/lab3
9. Start a new PHP/Apache container to serve the new app from the `/var/apps/lab3` directory:
		$ docker run --name php-lab3 -p 80:80 -v /var/apps/lab3:/var/www -d eboraas/apache-php
This command starts a new Apache/PHP container named php-lab3, linking its `/var/www` directory with our server's `/var/apps/lab3` directory, and also linking the container's port 80 with our server's port 80, so that all incoming requests on port 80 will be directed to our new php/apache container.
10. Place php/html application files in `/var/apps/lab3` directory.

At this point you will have a fully functioning LAMP stack with the AMP portion running in portable containers, which are almost entirely isolated from direct access from the internet. In practice you would not link port 80 of the host server to a single container directly, instead we would likely link it to some other non-standard port, or better yet, link it to a non-standard port on another container that is running an instance of a load balancing proxy such as haproxy. This configuration would allow us to run multiple copies of the same application, or just multiple applications on the same server, all of which are isolated from each other. Also note, it is not best practice to link a directory from the host machine into the container like we did in steps 8 and 9, instead we should have created another "volume" container to be used as storage, further isolating the applications from the host server. 

Although it may seem quite inefficient to run these services in this manner, it actually isn't! Docker does not use virtualization to accomplish containerization, instead it makes use of linux kernel namespaces and union file systems, allowing each container to execute natively using the host machine's kernel and storage, but have the execution and execution environment remain isolated from the main host machine's and all other container's.
