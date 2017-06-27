#  Setting up a simple firewall
Additional steps to improve the hot node is to install a service that restarts your masternode in case it crashes, and a firewall to help the networking performance and reduce the risk of malicious network activities. This small guide is for a Ubuntu server setup.

Let's get a simple firewall up and running:

	sudo apt-get install ufw

After installing, we need to setup some basic firewall rules. If something goes wrong, you can turn off the firewall with the command:

	sudo ufw disable

Please enter the firewall rules as shown below:

	sudo ufw allow ssh/tcp
	sudo ufw limit ssh/tcp
	sudo ufw allow 19683/tcp
	sudo ufw logging on
	sudo ufw enable

Verify the firewall setting with:

	sudo ufw status

	sudo ufw status
	Status: active

	To                         Action      From
	--                         ------      ----
	22/tcp                     LIMIT       Anywhere
	19683/tcp                  ALLOW       Anywhere
	22/tcp (v6)                LIMIT       Anywhere (v6)
	19683/tcp (v6)             ALLOW       Anywhere (v6)

Excellent, the firewall is all set!
