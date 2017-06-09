# A general Firewall on Digital Ocean

Digital Ocean has just released a simple firewall interface to help your droplets and protect the services from abuse. Note, that this firewall just is for keeping unwanted traffic off your VPS, and if more advanced configurations are needed, the user must look into setting up iptables or something similar instead.

Rather than rewriting the Digital Ocean guide, please see the link here how to setup and manage a simple firewall:

https://www.digitalocean.com/community/tutorials/how-to-create-your-first-digitalocean-cloud-firewall

Note that for a MonetaryUnit masternode, the ports 22 and 29888 will be required to be open for traffic on TCP.
