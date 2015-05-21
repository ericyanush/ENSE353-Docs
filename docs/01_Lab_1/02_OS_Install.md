Once the box has been built, [see here](Box_building.html), the machine requires an Operating System to be installed. In this case the lab instructor has chosen for us to install CENTOS 7, a RedHat derivative, on our machines.

###Requirements
----
1. A functioning machine to install the OS on. ([See here](Box_building.html))
2. A CD of the CENTOS 7 minimal install image.

###Installation
----
The steps required to achieve a fully function CENTOS 7 install are as follows:
1. Insert CD into CD drive.
2. Interrupt normal machine boot sequence. In the case of my machine, during the BIOS POST I had to press the Return Key, then press the F8 key to select a temporary boot device.
3. Select the CENTOS installer CD as the boot device.
4. Select the Install CENTOS 7 Option.
5. Allow the installer media to run through its series of self-tests
6. Once the Anaconda GUI installer launches, select English for the language and US for the keyboard layout
7. In the software selection screen, select Minimal Install
8. In the Installation Destination screen, select _I will configure partitioning_ to manually configure the drive partition map.
9. In the next screen, remove *ALL* partitions that are currently on the drive. (Some of the drives were previously used for other purposes or previous lab sections)
10. Select the option to enable LVM (Logical Volume Management) partitioning scheme, and select the option for the installer to automatically create partitions and mount points.
11. When prompted, select Accept to accept the destructive changes made to the partition map
12. Select the button at the bottom right of the installer screen to begin the installation.
13. During the installation process, you are prompted to create a root user password, one should be used that is sufficiently immune to dictionary attacks as this box will be a server that will be open to the internet and various malicious attacks.
14. Along with a root user password, you are prompted to create another user, you should create a new user for yourself, with administrator privileges (prevents having to use root user to  make all system changes), again with a sufficiently strong password.

At this point, the installation will complete autonomously, and after completed, the installer will prompt you to restart the machine, after removing the installer media from the CD drive. Once the machine completes its reboot cycle, it will boot to a command line login screen. After successfully authenticating with your login credentials, you will be presented with a [BASH](www.gnu.org/software/bash/) shell prompt. At this point this OS install is complete and fully functioning.