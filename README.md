SAMBA Server on GCE VM (Ubuntu 16.04 LTS Or Above)

# Installing Samba

$ sudo apt update
$ sudo apt install samba

# Setting up Samba

Now that Samba is installed, we need to create a directory for it to share:

$ mkdir /home/<system-username>/sambashare/

The command above creates a new folder samba share in our home directory which we will share later.The configuration file for Samba is located at /etc/samba/smb.conf. To add the new directory as a share, we edit the file by running:

$ sudo nano /etc/samba/smb.conf

At the bottom of the file, add the following lines:
	
 [sambashare]
     comment = Samba on Ubuntu
     path = /home/<system-username>/sambashare
     read only = no
     browsable = yes
     guest = yes
     users = all
     admins = no
	
Then press Ctrl-O to save and Ctrl-X to exit from the nano text editor.
Now that we have our new share configured, save it and restart Samba for it to take effect:

$ sudo service smbd restart

Update the firewall rules to allow Samba traffic:
$ sudo ufw allow samba

# Setting up User Accounts and Connecting to Share

Since, Samba doesn’t use the system account password, we need to set up a Samba password for our user account:
$ sudo smbpasswd -a username

Note: Required Port for SAMBA server Communication is TCP=139,445 Needs to be open. 

# Testing 

On Ubuntu or MacOS: Open up the default file manager and click Connect to Server then enter:
smb://ip-address/sambashare

Windows: On Windows, open up File Manager and edit the file path to:
\\ip-address\sambashare

Note: Make sure you add an A record for SAMBA server(in a private hosted zone) because of the Kubernetes SMB CSI Driver requirement.

Reference Link:

1)	https://cloud.google.com/kubernetes-engine/docs/how-to/access-smb-volume
2)	https://github.com/kubernetes-csi/csi-driver-smb/tree/master/deploy/example/smb-provisioner
