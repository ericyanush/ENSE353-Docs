The first exercise assigned for the seventh lab was to install GUI on our servers.

###Requirements
------------------------

The requirements to install the GUI are:
1. Functioning Lab Machine

###Installing a GUI
----------------

To setup the KDE GUI, the following steps were taken:

1. Install KDE:
		$ yum groupinstall "KDE Plasma Workspaces"
2. Reboot:
		$ shutdown -r now
3. Start the GUI:
		$ startx

At this point you will have a fully functioning Desktop GUI installed on your server. This guide assumes that you want to use KDE, if you are like me, you may like/prefer GNOME instead. To install GNOME, in step 1, run:
		$ yum groupinstall "GNOME Desktop"
instead of the command listed above. Note that this will default to starting a GNOME Classic session, if you would rather start a GNOME Shell session instead, which is recommended, also execute the following command:
		$ echo "exec gnome-session" >> ~/.xinitrc
before running the `startx` command listed in step 3.