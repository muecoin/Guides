# Building the MonetaryUnit daemon for CentOS 7

Here's a simple how-to for building and installing the MonetaryUnit daemon (mued) for a CentOS 7 server system. We will build the daemon without graphical support, as this is intended for a headless server.

We need to install some additional packages from the `EPEL repository` to build the MonetaryUnit daemon.

_From the fedoraproject.org:
What is Extra Packages for Enterprise Linux (or EPEL)?
Extra Packages for Enterprise Linux (or EPEL) is a Fedora Special Interest Group that creates, maintains, and manages a high quality set of additional packages for Enterprise Linux, including, but not limited to, Red Hat Enterprise Linux (RHEL), CentOS and Scientific Linux (SL), Oracle Linux (OL).
EPEL packages are usually based on their Fedora counterparts and will never conflict with or replace packages in the base Enterprise Linux distributions. EPEL uses much of the same infrastructure as Fedora, including buildsystem, bugzilla instance, updates manager, mirror manager and more._

### Installing the EPEL repository
For a x86_64 based CentOS 7 system:
In the console, log in as root and run the setup commands:

    yum update && yum upgrade
    wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
    yum install epel-release-latest-7.rpm

### Installing the support packages and dependencies

    yum install autoconf automake gcc-c++ libdb4-cxx libdb4-cxx-devel boost-devel openssl-devel libtool libevent-devel

### Get the MonetaryUnit source for building

Down load the source and unpack the file.

    wget https://monetaryunit.org/download/linux64-1.0.0.1.tar.gz
    tar -zxvf linux64-1.0.0.1.tar.gz

### Configure and compile the source

    cd MUE/linux64-1.0.0.1
    ./autogen.sh && ./configure --with-incompatible-bdb --without-miniupnpc && make

### Run the daemon and check the MUE console

    cd src/
    ./mued -daemon
    MonetaryUnit Core server starting

    ./mue-cli getinfo
    {
      "version": 1000001,
      "protocolversion": 70206,
      "walletversion": 61000,
      "balance": 0.00000000,
      "privatesend_balance": 0.00000000,
      "blocks": 231,
      "timeoffset": 0,
      "connections": 1,
      "proxy": "",
      "difficulty": 0.0002441371325370145,
      "testnet": false,
      "keypoololdest": 1495447627,
      "keypoolsize": 1001,
      "paytxfee": 0.00000000,
      "relayfee": 0.00010000,
      "errors": ""
    }

That's it! Congratulations on setting up the MonetaryUnit daemon on a CentOS 7 system!
