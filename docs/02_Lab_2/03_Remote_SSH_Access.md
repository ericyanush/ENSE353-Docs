The third exercise assigned for the second lab was to ensure you are able to remotely access your machine.

###Requirements
------------------------

The requirements for remote access are:
1. Completion of the [connect to internet exercise.](Connecting_Network.html)
2. Another machine that has internet access. (Preferable a UNIX machine)
3. Public ip address of your machine, found in the [internet connection exercise.](Connecting_Network.html)

###Remote Access
----------------

To test for remote access the following steps were taken:
1. On the other (remote) machine issue the command as follows:
		$ ssh <your username>@<your public ip address>
2. When prompted accept the RSA fingerprint of the unknown machine, accept it.
	<img src="../img/Lab_2/rsa_fingerprint.png" alt="editing the interface script in vi" class="img-responsive"  style="margin-bottom:20px;height:350px;margin-right:auto;margin-left:auto;">
3. Enter your password when prompted.
You should now have remote access to your machine. You can check at any time who is logged into your machine, either locally or remote, with the following command:
		$ who -a
<img src="../img/Lab_2/remote_login.png" alt="editing the interface script in vi" class="img-responsive"  style="margin-bottom:20px;height:350px;margin-right:auto;margin-left:auto;">
