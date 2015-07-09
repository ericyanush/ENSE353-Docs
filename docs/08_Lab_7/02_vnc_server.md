The second exercise assigned for the seventh lab was to install a vnc viewer on our servers, so that we can remotely control a desktop session.

###Requirements
------------------------

The requirements to install the SquirrelMail server are:
1. Functioning Lab Machine
2. Installed [GUI](gui.html)

###Installing TigerVNC
----------------

To install TigerVNC on your server:

1. Install TigerVNC:
		$ yum install tigervnc-server
2. Allow VNC traffic through the firewall, default to port 5901:
		$ firewall-cmd --zone=public --add-port=5901/tcp --permanent
		$ firewall-cmd --reload
3. Start the vnc viewer on default port:
		$ vncserver :1
4. Enter a password to secure the session from the outside world, when prompted.

At this point you will have a vnc server running its own unique desktop GUI session, and have it available for viewing/control on port 5901. If you install GNOME, as I did, for a GUI and would like GNOME Shell to be launched for the VNC sessions as well, you will need to modify the file `~/.vnc/xstartup`. You will need to comment our line #5, and underneath it add: `exec gnome-session`.
<img src="../img/Lab_7/xstartup.png" alt="select predefined options" class="img-responsive"  style="margin-bottom:20px;height:350px;margin-right:auto;margin-left:auto;">