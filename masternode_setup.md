# MonetaryUnit Masternode Setup

	A MUE guide for setting up and hosting a masternode on a VPS (virtual private server)
	Version 0.0 Initial draft 2017-05-17

The MUE network has switched to a Dash-X11 code base, and as such it inherits the strengths that a 2-tier network brings, in terms of a strong masternode network for additional functions and security of the network.

The masternodes perform tasks for the MUE network. These are for instance InstantSend, PrivateSend, storage of the blockchain and governance capabilities. As a reward for running these functions, the masternodes are paid out a part of the block reward for their services. The block reward is 40 MUE per block and each block occurs on average every 40 seconds, thus growing the supply 1 MUE every second.

The block reward is split in three providing 45% to the miner who solved the block, 45% to the masternode for it’s services and the remaining 10% going to the continuous funding of the MUE project. The masternode reward is 45%*40 MUE = 18 MUE per block.

## How to setup a Virtual Private Server (VPS) and run a MUE masternode.

For starters, the reason for running a dedicated VPS instead of hosting one’s masternode on a home internet connection are several. The masternode must be up and running all the time to provide services for the network. Should the connection go down, or the node performs badly, the rest of the network will be unhappy with the services and the masternode loses payments. Many home internet providers are unhappy if the owner runs a server at home and may shut down access to the masternode.

A typical masternode uses around 500 GB or more of traffic per month. Home internet users with traffic caps will have a hard time running a masternode if the bandwidth is limited. It is especially important to have a high speed connection as the masternode is serving up the blockchain. Additional feature to the network will likely require a high speed connection as well.

### VPS requirements for a masternode
The minimal requirements for running a masternode are:

	* 1 GB of ram
	* 1 CPU core
	* 20 GB of harddrive space
	* a high speed internet connection and at least a transfer of 500 GB per month.

Better hardware can be used to help the masternode performance, but using less than the required minimal hardware can make the masternode unstable and cause the node to crash or perform poorly. By doing so, the node can get kicked off the masternode lists, causing the masternode to lose payments.


### Setting up a local wallet for cold storage and a hot wallet (masternode) on a VPS

Let's start by getting the wallet for your platform.
Please go the website: https://monetaryunit.org/download/ and select the wallet that matches your computer. For this tutorial we will be using a 64-bit Windows machine for the cold storage wallet and a Linux VPS for the hot masternode daemon.

#### Install wallet on local machine
Download the Windows client, monetaryunit-1.0.10.8-win64-setup.exe. You may check the files with the provided checksums, verify that the files downloaded correctly, and if they are different, please redownload the client again.

#### Install the client, and then start it.
You may be prompted by the Windows Firewall to allow the wallet to accept connections on private and open networks. Please select both network boxes and accept. Now, with the client open, let it sync to the network wait for the blockchain to be downloaded. The wallet will attempt to synchronize with the network, no manual actions should be needed at this point.
<a href="Images/00-mue-wallet-data-dir.PNG"><img src="Images/00-mue-wallet-data-dir.PNG" width="400" ></a>
<a href="Images/01_mue_wallet_start_screen.PNG"><img src="Images/01_mue_wallet_start_screen.PNG" width="400" ></a>

#### Protect your funds with a strong password.
In your wallet, go to `Settings` and then `Encrypt Wallet`
Provide a strong password to secure your funds. Please make sure to remeber this password or write it down and keep it in a bank safe deposit. If you lose your wallet password, there is no means of recovering your funds!
After providing a strong password, the wallet needs to be restarted to lock and encrypt your funds. Please restart your wallet for this step to complete.
<a href="Images/02_encrypt_wallet.PNG"><img src="Images/02_encrypt_wallet.PNG" width="400" ></a>
<a href="Images/03-encrypt-confirm.PNG"><img src="Images/03-encrypt-confirm.PNG" width="400" ></a>
<a href="Images/04-encrypt-done-restart-wallet.PNG"><img src="Images/04-encrypt-done-restart-wallet.PNG" width="400" ></a>

We need to activate a few advanced wallet features before proceeding.
Click on `Settings` and `Options` and select the two top boxes

	"Enable coin control features" and "Show Masternodes Tab"

Click `OK` and we are ready to continue.
![wallet password](./Images/07-options-mastenode-tab-coincontrol.PNG)

With the wallet back up and running, it's time to get it ready for the masternode address.

Open the console by clicking the `Tools` menu and selecting `Debug console`
The next command provides us with a key for the masternode collateral. Please type

	masternode genkey

and hit enter. The output will look something like this:

	7qm78BkY2LvriqGVTwRoXJWRTZkm9yH79L2FbRTp9NrQaTKRcGZ

![wallet password](./Images/12-masternode-genkey.PNG)

Note: this is not the same as a MUE address private key. This key is used to communicate to the MUE netork that this is a masternode, it cannot be used to move the collateral funds. The key is used to announce the precence of the masternode to the network, or remove the node from the mastrenode list.

Next, we need to make a transaction of exactly 500 000 MUE to an address. This is very important, the transaction must be exaclty 500 000 MUE for the collateral, no more and no less.
Go to the `Recieve` tab of your wallet and in the Label field write `Masternode Collateral` and keep the two checkboxes `Reuse an existiing recieving address` and `Request InstantSend` unchecked. Click the orange button `Request payment` to get a new address:

	7WszYALmCVMJLN39JngawyVszvwrjYMEub

This is the addtress we will be sendig our collateral to.

Next, make a transaction of exactly `500 000` MUE to the adddress you just generated. If you are sending from you own wallet or from an exchange, make sure that any transaction fees are not taken from the sending amount. It is crucial that the collateral transaction is exactly `500 000` MUE and made in one (1) single transaction.

Go to the `Send` tab of your wallet and paste in the MUE address to recieve the collateral funds.
Uncheck the box with `Subtract fee from amount` and ensure that the sending amount is exactly `500 000` MUE.

Please tripple check that the address is correct, and that the sending amount is `500 000` MUE. If you by mistake send your funds to the wrong address, *THEY WILL BE GONE!*

After checking everything, send your funds to the address above and wait for the transaction to confirm. Press `send` and write your wallet password to make the transaction.
Check the `Transactions` tab, and there should be a transaction listed as `Payment to yourself` if you sent the funds from the wallet. Otherwise sending from an exchange will list that transaction address instead.

To avoid spending these funds we can `lock` the transaction in the wallet. This is done with the the coin control features of the wallet. Go back to the `Send` tab of the wallet and click on the `Inputs...` botton.
Select the box(es) with the `500 000` MUE transfer(s). Right click and chose `Lock unspent`. Notice that a small lock icon is shown next to the tranaction. This prevents the coins in this transaction from being spent the next time a transaction is made from the wallet. This is good, because if funds are taken from the holding address, it breaks the transaction for the masternode collateral and the masternode stops working - even if we have more than `500 000` MUE in our cold storage wallet! Should you want to send these funds again, simply right click on the transaction and select `Unlock unspent`.

#### While waiting, let's start working on the VPS (Virtual Private Server) hot node.
This guide will use Digital Ocean as a VPS provider, but the reader is free to chose whomever they like. The auther has used them for years as a masternode host and they are very reliable. There are definatly cheaper providers, but something to consider is that the masternode needs to be up and running 24/7 for supporting the network. Running a masternode on a cheaper, less reliable host will likely give more downtime and less payments in the longer run.

This promotion&referral code `https://m.do.co/c/0c50700df3bf` gives every new user a `$10` value to start using a new VPS. Please freel free to use it if you like.
Sign up for a new account, log in and click `New Droplet`
For this guide we are looking to start a `16.04 Ubuntu` version of Linux as the host:
Select the `1 GB / 1 CPU, 30 GB SSD Disk, 2 TB Transfer` version. The $10 USD credit provided via the promotional code will give most of the first month hosting for free! Chose a location, it doesn't really matter, but it might be better to chose a location closer to yourself for the remote management.

Create the droplet, and the `root` login, `ip` and `password` will be sent to your email address. These details are needed to access your VPS instance.

To access a Linux VPS, you need a program called Putty, `http://www.putty.org/` it is a client to securely connect remotly to the VPS host.

Install the client and open putty.exe, if needed allow it through the Windows firewall and access your new droplet.
Provide the ip-number, keep the port on 22, and save the configuration with a suitable name. This allows for easier access next time you want to log on your droplet.
Click connect and a block window will open up, promting you for your login and password. `root` is the login and the password is in your email from Digital Ocean.


Once logged in, it is smart to change the root password to something more secure. Remeber that the email with the root passowrd was sent in plain text, better change that and secure your VPS!
Type the command:

	passwd

and then provide a new password twice to change the password for user root.

We need a new common user to run the masternode. Running as root is in itself a security risk and is not encouraged. Here we are adding the user "mue", but you may choose whatever username you like.
type the following commands:

	useradd -d /home/mue -s /bin/bash -m mue
	passwd mue

This adds the user `mue` and we setup the users password.
Next add the user to the sudo group:

	usermod -aG sudo mue

Now, we have a normal user that can do some administrative tasks on the system.

Lets switch to the new user `mue` and proceed with the installation:

	su - mue

Just for good meassures, make sure the user mue is in the right directory:

	cd /home/mue

Before starting the installation, make sure the VPS is all updated and ready to go. Do upgrade the VPS from time to time. Once a week is probably a good routine:

	sudo apt-get update && sudo apt-get upgrade && sudo apt autoclean

Select `y` to proceed with any updates and wait for the VPS to complete the tasks.

Next, download the Linux client, and unpack the compressed files:

	wget https://www.monetaryunit.org/downloads/linux64-1.0.0.1.tar.gz
	tar -zxvf linux64-1.0.0.1.tar.gz

We need to copy the binaries to a local folder

	mkdir /home/mue/bin
	mkdir /home/mue/.muecore
	cp /home/mue/linux64-1.0.0.1/bin/64/mue* /home/mue/bin/

and we will make sure they are executable

	chmod 755 /home/mue/bin/mue*

We'll make sure that the binaries are found in the mue users path:

	export PATH=$PATH:$HOME/bin

this setting will not take place untill the user mue has logged in again.
Before proceeding it is best to log off, and log in to the VPS again. This time though, we can log in as user `mue` with that users password that was setup ealier.

-------------------------------------------------------------------
If we want to build the daemon instead, we have to setup the building environment before compiling the source:
Please note that compiling the software requires `more than 1GB of RAM memory` for the VPS. Please setup a `swap file` or use a VPS instance with at least `2GB of RAM memory`.

	sudo apt-get install build-essential libtool autotools-dev automake pkg-config libssl-dev libevent-dev bsdmainutils -y
	sudo apt-get install libboost-system-dev libboost-filesystem-dev libboost-chrono-dev libboost-program-options-dev libboost-test-dev libboost-thread-dev -y
	sudo apt-get install libboost-all-dev -y
	sudo add-apt-repository ppa:bitcoin/bitcoin -y
	sudo apt-get update -y
	sudo apt-get install libdb4.8-dev libdb4.8++-dev -y

	cd Mue
	./autogen.sh && ./configure --with-incompatible-bdb --without-miniupnpc && make
	cd src
	cp mued /home/mue/bin/
	cp mue-cli /home/mue/bin/
	cp mue-tx /home/mue/bin/
--------------------------------------------------------------------

Open putty again, and connect to your VPS. When prompted log in as user `mue`

Next step is to setup the configuration files for the MUE daemon (client)
Use the editor nano to edit the files. More experienced users may use other editors, but nano is an easy to use editor for new beginners and more experienced users alike.

	cd .muecore
	nano -w mue.conf

write or copy the settings below to your `mue.conf` file:

	rpcuser=XXXXXXXXXXXXX
	rpcpassword=XXXXXXXXXXXXXXXXXXXXXXXXXXXX
	rpcallowip=127.0.0.1
	#----
	listen=1
	server=1
	daemon=1
	maxconnections=50
	#--------------------
	masternode=1
	masternodeprivkey=XXXXXXXXXXXXXXXXXXXXXXX
	externalip=XXX.XXX.XXX.XXX

The `rpcuser` and `password` can be anything you like, just put in some long random strings of text.
The `masternodeprivkey` is the long key we obtained earlier on the Windows wallet after the `masternode genkey` command.
The `externalip` is the `ipnumber` of the `masternode VPS`.

Save and exit the edited file by pressing `ctrl w` followed by `ctrl x`.
We will secure the conf file by changing its permissions:

	chmod 600 /home/mue/.muecore/mue.conf

# Start the mue daemon

	mued -daemon

The MUE client will start downloading the blockchain, and syncronize to the network. Give it time to sync up, and in the meantime we will continue with more configuratin on the cold node (Windows). The `masternode.conf` on the Windows host must be made with the correct data for controlling the masternode hot node.

Open the Windows MUE client again, and under the menu `Tools` look for the option `Open Masternode Configuration File` near the bottom of the list. Select that option to open up the configuration file. If prompted, select to open the file in Notepad.
The syntax of this file is very important, but it is also very straight forward.

The masternode.conf file needs a few values;

	Format: alias IP:port masternodeprivkey collateral_output_txid collateral_output_index

* the `alias` is a name we assign to our masternode, like `MN1`
* `IP:port` are the external ip and node for the hot node on the VPS. The port for MUE mainnet is `29988`
* `masternodeprivkey` is the output from the `masternode genkey` command earlier, eg `7qm78BkY2LvriqGVTwRoXJWRTZkm9yH79L2FbRTp9NrQaTKRcGZ`
* for the `collateral_output_txid` and `collateral_output_index` we need to issue a command in the cold wallet console.

In the wallet, select `Tools` and `Debug console`, and in the console window run the command:

	masternode outputs

the output will look something like this:

	masternode outputs
	{
	  "f9a2013b00205098ad8b192d9ac68e7d55e47eb80c860972a404a51bfbc100ac": "1"
	}

which are the two values that we are looking for. The `collateral_output_index` will be either a `0` or a `1`.

Put these values in the `masternode.conf` all on one single line, save the file and restart the wallet.

	MN1 123.45.56.78:29988 7qm78BkY2LvriqGVTwRoXJWRTZkm9yH79L2FbRTp9NrQaTKRcGZ f9a2013b00205098ad8b192d9ac68e7d55e47eb80c860972a404a51bfbc100ac 1

When the cold wallet starts up again, check the masternode tab, and the masternode should be visable in the list of your own masternodes.

## Final last commands on the VPS again, and let's check the status of the masternode.

	mue-cli mnsync status

We need the masternode hot wallet to be synced and up to date before proceeding.
It will be ready when the output looks like this:

	{
	"AssetID": 999,
	"AssetName": "MASTERNODE_SYNC_FINISHED",
	"Attempt": 0,
	"IsBlockchainSynced": true,
	"IsMasternodeListSynced": true,
	"IsWinnersListSynced": true,
	"IsSynced": true,
	"IsFailed": false
	}

If the output is different, please wait for the node to finnish syncing, and try the command `mue-cli mnsync status` again
When the hot node is all synced and ready, the node is communicating to the MonetaryUnit network and waiting for further instructions. The masternode is not yet accepted by the other masternodes, we must start it first.

We need to activate the masternode, so we move back to the cold wallet on Windows.
Check the masternode tab of the wallet.

If you have more than one masternode in the list, check the masternode that you would like to start.

Finally, click the `Start alias` button, and the masternode will be activated on the network. You will be promted to unlock your wallet, so type in your password and click `OK`.

We can see the output change in the masternode status fields to `Pre-Enabled` and then `Enabled`

Veryify the status on your hot node with the command:

	mue-cli masternode debug
	Masternode successfully started

Well done!
Congratulations to successfully getting your masternode up and running!

---------------------------------------------------------------------------
## Setting up a simpe firewall
Additional steps to improve the hot node is to install a service that restarts your masternode in case it crashes, and a firewall to help the ntworking performace and reduce the risk of malicious network activities.

Let's get a simple firewall up and running:

	sudo apt-get install ufw

After installing, we need to setup some basic firewall rules. If something goes wrong, you can turn off the firewall with the command:

	sudo ufw disable

Please enter the firewall rules as shown below:

	sudo ufw allow ssh/tcp
	sudo ufw limit ssh/tcp
	sudo ufw allow 29988/tcp
	sudo ufw logging on
	sudo ufw enable

Verify the firewall setting with:

	sudo ufw status

	sudo ufw status
	Status: active

	To                         Action      From
	--                         ------      ----
	22/tcp                     LIMIT       Anywhere
	29988/tcp                  ALLOW       Anywhere
	22/tcp (v6)                LIMIT       Anywhere (v6)
	29988/tcp (v6)             ALLOW       Anywhere (v6)

Excellent, the firewall is all set!

------------------------------------------------------------

## Installing the monit service to auto restaret the MUE daemon in case it crashes.
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

That's all folks! The monit service will keep your masternode up and running it is crashes for whatever reason. The monit service checks once aminute if the mued process is running, and restarts it, if it is not.

------------------------------------------------------------

## Prevent root from loggin in via ssh for increased security.

One of the most common ways that people try to take over a Linux host, is by trying to login in as the root user. One simple way to increase the security of the Linux VPS is to simply remove this access, as we have another normal user to use instead (user mue in the example above).

Start by logging into the VPS, and edit the `sshd_config` file:

	sudo nano -w /etc/ssh/sshd_config

Scroll down and look for the row `PermitRootLogin yes`  and change to `no`
Additionally, we can specify who may connect by adding the line, where `mue` is the user account on the VPS:

	AllowUsers mue

then we may save the configuration by `ctrl o`and `ctrl x`

That's it! The next time you need to access your VPS, login with the user you specified. Your VPS is now secured from users trying to bruteforce the root remote login.
