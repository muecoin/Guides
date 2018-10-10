# Setting up a Masternode on the update Monetary PoS network

sudo apt-get update && sudo apt-get upgrade -y
wget https://github.com/muecoin/MUE/releases/download/v2.0.2/mon-x86_64-linux-gnu.tar.gz
tar -zxvf mon-x86_64-linux-gnu.tar.gz
sudo cp mon/bin/monetaryunit* /usr/local/bin/
monetaryunitd --daemon
nano -w .monetaryunit/monetaryunit.conf

	rpcuser=XXXXXXXXXXXXXXXXXXXXXXX
	rpcpassword=XXXXXXXXXXXXXXXXXXXXXXXXXXXX
	rpcallowip=127.0.0.1
	#----   
	listen=1
	server=1
	daemon=1
	maxconnections=256 
	#--------------------
	masternode=1
	masternodeprivkey=XXXXXXXXXXXXXXXXXXXXXXX
	externalip=XXX.XXX.XXX.XXX:19687

ctrl +o ctrl +q to save the file

monetaryunit-cli stop
monetaryunitd --daemon    starts the daemon again
