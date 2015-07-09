The third exercise assigned for the seventh lab was to install the virtualbox virtual machine environment.

###Requirements
------------------------

The requirements to install VirtualBox are:
1. Functioning Lab Machine
2. Installed [GUI](gui.html)

###Installing VirtualBox
----------------

To install VirtualBox on your server:

1. Install the Kernel sources package, so that the kernel extensions (modules) can be built for virtualbox:
		$ yum install kernel-devel
2. Install the development tools packages, so that we will have compilers and build systems available to build the aforementioned Kernel extensions:
		$ yum groupinstall "Development Tools"
3. Install the VirtualBox package repository into yum:
		$ cd /etc/yum/repos.d
		$ curl -O http://download.virtualbox.org/virtualbox/rpm/el/virtualbox.repo
4. Install VirtualBox:
		$ yum install virtualbox-4.3

At this point virtualbox will be installed and ready to create new virtual machine instances on your server. Note: When a Kernel update is installed on your server, virtualbox will require its modules be rebuilt for the newly installed Kernel. This can accomplished with the command:
		$ /etc/init.d/vboxdrv setup
