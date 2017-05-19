# Installing the monit service to auto restart the MUE daemon in case it crashes.

Remember that the masternode needs to be up and running 24/7 to provide its services to the MonetaryUnit network. If it goes offline the masterndoe will stop being paid for it's services and lose its place in the payment queue.

Let's install monit:

	sudo apt-get install monit

Create a small file to start the mued

	nano -w /home/mue/start-mued.sh

and add the command below to the file:

	#!/bin/bash
	/bin/su mue -c '/home/mue/bin/mued -daemon 2>&1 >> /home/mue/.muecore/rc.local.log'

save and close nano with `ctrl o` and `ctrl x`

Make the helper file executable:

	chmod 755 /home/mue/start-mued.sh

Edit the file /etc/monit/monitrc

	sudo nano /etc/monit/monitrc

Edit the file as follows:

uncomment these lines

	set httpd port 2812 and
	use address localhost  # only accept connection from localhost
	allow localhost        # allow localhost to connect to the server and

add this to bottom - change user to your user and paths

	check process mued with pidfile /home/mue/.muecore/mued.pid
	start program = "/home/mue/start-mued.sh" with timeout 60 seconds
	stop program = "/bin/su mue -c /home/mue/bin/mue-cli stop"

Load the new configuration:

	sudo monit reload

Enable the monitoring service:

	sudo monit start mued

That’s it. You only have to do the above once.
You can check monit's status with the command:

	sudo monit status

That's all folks! The monit service will keep your masternode up and running it is crashes for whatever reason. The monit service checks once a minute if the mued process is running, and restarts it, if it is not.
